# Data Pipeline Pattern

## Pattern Name
ETL Data Pipeline

## VIBES Rating
<👓🪢💧>

## Description
Traditional Extract-Transform-Load pipeline with rigid sequential processing. Each stage must complete before the next begins. Represents functional but inflexible data architecture.

## Code Example
```python
class ETLPipeline:
    def __init__(self, source_db, target_db):
        self.source = source_db
        self.target = target_db
        self.extracted_data = None
        self.transformed_data = None
    
    def extract(self, query):
        """Extract data from source - must complete first"""
        try:
            self.extracted_data = self.source.execute(query)
            print(f"Extracted {len(self.extracted_data)} records")
            return self
        except Exception as e:
            raise ExtractError(f"Extraction failed: {e}")
    
    def transform(self, rules):
        """Transform data - requires extracted_data"""
        if self.extracted_data is None:
            raise PipelineError("No data to transform")
        
        try:
            self.transformed_data = []
            for record in self.extracted_data:
                transformed = apply_rules(record, rules)
                self.transformed_data.append(transformed)
            return self
        except Exception as e:
            raise TransformError(f"Transformation failed: {e}")
    
    def load(self):
        """Load to target - requires transformed_data"""
        if self.transformed_data is None:
            raise PipelineError("No data to load")
        
        try:
            self.target.bulk_insert(self.transformed_data)
            print(f"Loaded {len(self.transformed_data)} records")
        except Exception as e:
            raise LoadError(f"Load failed: {e}")

# Usage - rigid sequence
pipeline = ETLPipeline(source_db, target_db)
pipeline.extract("SELECT * FROM orders") \
        .transform(cleaning_rules) \
        .load()
```

## Why This Rating?

### Expressive Power: 👓 (Readable)
- One way to process: Extract → Transform → Load
- Method chaining provides single path
- No flexibility in ordering

### Context Flow: 🪢 (Pipeline)
- Strict linear dependencies
- Each stage requires previous completion
- State passed through pipeline stages

### Error Surface: 💧 (Liquid)
- Runtime exceptions at each stage
- Try-catch error handling
- Failures discovered during execution

## Anti-Pattern Version
<🙈🌀🌊>
```python
# Global state chaos
data = None
errors = []

def process_data(config_string):
    global data, errors
    
    # Parse config from string
    parts = config_string.split('|')
    
    # Everything affects everything
    if 'extract' in parts[0]:
        data = eval(parts[1])  # Security nightmare
    
    if data and 'transform' in parts[2]:
        # Mutates global data
        for i in range(len(data)):
            data[i] = transform_somehow(data[i])
    
    # Errors cascade unpredictably
    if not data:
        errors.append("Something went wrong")
```

## Crystalline Version
<🔬🎀💠>
```haskell
-- Type-safe streaming pipeline with compile-time guarantees
data Pipeline source target where
  Extract :: Connection source -> Query -> Pipeline source [Row source]
  Transform :: (Row source -> Validation (Row target)) -> Pipeline [Row source] [Row target]
  Load :: Connection target -> Pipeline [Row target] LoadResult
  
-- Combinators ensure valid composition
(>>=>) :: Pipeline a b -> Pipeline b c -> Pipeline a c

-- Usage with compile-time verification
processOrders :: Pipeline OrderDB ReportDB
processOrders = 
  Extract sourceConn ordersQuery >>=>
  Transform validateAndEnrich >>=>
  Load targetConn

-- Invalid combinations won't compile
-- Transform validateAndEnrich >>==> Extract sourceConn  -- Type error!
```

## Boundary Demonstration: 🪢 vs 🧶

### This Pattern (🪢 - Pipeline)
- Dependencies are LINEAR: A → B → C
- Changing one stage doesn't affect others' internals
- Clear interfaces between stages

### Coupled Version (🧶)
```python
class CoupledETL:
    def process(self):
        # Everything tangled together
        data = self.source.query()
        
        # Transform knows about extract details
        if self.source.type == "postgres":
            data = postgres_transform(data)
        elif self.source.type == "mysql":
            data = mysql_transform(data)
        
        # Load knows about transform details
        if hasattr(data[0], 'customer_id'):
            self.target.customer_table.insert(data)
        else:
            self.target.order_table.insert(data)
```

## Context Considerations
- For batch processing: 👓🪢💧 is often sufficient
- For streaming: Need reactive patterns <🔍🎀🧊>
- With type safety: Can achieve <🔬🪢💠>
- In notebooks: Often devolves to <🙈🧶💧>

## Related Patterns
- [[streaming-pipeline]] - For reactive data flow
- [[parser-combinators]] - Similar composition pattern
- [[validation-chain]] - For data validation

## References
- Apache Beam's unified model
- Spark's RDD transformations
- Data engineering best practices
