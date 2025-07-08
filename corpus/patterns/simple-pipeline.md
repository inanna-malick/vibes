# Pattern: Simple Pipeline

## VIBES Rating
`<👓🪢🧊>`

## Description
A basic linear data transformation pipeline with clear stages but limited flexibility. Each step transforms data predictably, errors fail at startup, but the rigid structure limits expressiveness.

## Example
```python
# Simple data processing pipeline with startup validation
class DataPipeline:
    def __init__(self, config):
        # Validation at initialization
        self.validate_config(config)
        
        # Fixed pipeline stages
        self.stages = [
            self.clean_data,
            self.validate_schema,
            self.transform_values,
            self.aggregate_results
        ]
        
        # Test pipeline at startup with sample data
        try:
            self.test_pipeline()
        except Exception as e:
            raise RuntimeError(f"Pipeline initialization failed: {e}")
    
    def validate_config(self, config):
        required = ['input_path', 'output_path', 'schema']
        for field in required:
            if field not in config:
                raise ValueError(f"Missing required config: {field}")
    
    def test_pipeline(self):
        """Run pipeline with test data to ensure all stages work"""
        test_data = {"test": "data"}
        for stage in self.stages:
            test_data = stage(test_data)
    
    def process(self, data):
        """Process data through fixed pipeline stages"""
        result = data
        for stage in self.stages:
            result = stage(result)
        return result
    
    # Fixed stage implementations
    def clean_data(self, data):
        # Remove nulls, trim strings, etc.
        return {k: v for k, v in data.items() if v is not None}
    
    def validate_schema(self, data):
        # Check required fields exist
        if 'id' not in data:
            raise ValueError("Missing required field: id")
        return data
    
    def transform_values(self, data):
        # Fixed transformations
        if 'name' in data:
            data['name'] = data['name'].upper()
        return data
    
    def aggregate_results(self, data):
        # Simple aggregation
        data['processed'] = True
        data['timestamp'] = datetime.now()
        return data

# Usage
pipeline = DataPipeline(config)  # Fails at creation if invalid
result = pipeline.process(input_data)  # Linear processing
```

## Why This Rating

### Expressive Power: 👓
- Clear purpose: sequential data transformation
- Only one way to process: through fixed stages
- No flexibility in stage ordering or selection
- Can't add custom transformations without modifying class

### Context Flow: 🪢
- Perfect linear flow: data → clean → validate → transform → aggregate
- Each stage depends only on previous output
- No hidden state or side effects
- Can trace data flow easily

### Error Surface: 🧊
- Configuration validated at initialization
- Pipeline tested at startup with sample data
- Missing requirements fail immediately
- Runtime errors still possible but less likely

## Common Occurrence
- ETL scripts
- Data validation pipelines
- Report generation systems
- Batch processing jobs
- Simple API middleware chains

## Impact
- LLMs can easily follow the linear flow
- Modifications require understanding fixed structure
- Good for stable, well-defined processes
- Poor for dynamic requirements

## Related Patterns
- [promise-pipeline.md](./promise-pipeline.md) - Similar flow, more flexible
- [parser-combinators.md](./parser-combinators.md) - Composable alternative
- [builder-pattern.md](./builder-pattern.md) - More flexible construction

## Consensus Data
- Model Agreement: 91%
- Disputed Aspects: Some models rate 🔍 when stages are well-documented
- Test Date: July 2025
