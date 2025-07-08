# Validation Chain Pattern

## Pattern: <🔍🪢💧>

```python
def process_user_input(data: dict) -> ProcessedData:
    try:
        validated = validate_schema(data)
        normalized = normalize_fields(validated)
        enriched = add_defaults(normalized)
        return ProcessedData(enriched)
    except ValidationError as e:
        log_error(e)
        raise UserError(f"Invalid input: {e}")
```

## VIBES Analysis

- **Expressive** (🔍): Multiple valid pipeline arrangements, clear data flow
- **Context** (🪢): Linear dependency chain, each step depends on previous
- **Error** (💧): Runtime validation with explicit error handling

## Why This Rating

The validation chain shows structured flexibility - you could reorder some steps (normalization before validation in some cases) but the pipeline enforces a clear sequence. Errors are handled at runtime but contained through try-catch boundaries.

## Anti-Pattern Version

```python
# <👓🌀🌊>
def process(d):
    global errors
    if 'name' in d: d['name'] = d['name'].strip()
    if not d.get('age'): d['age'] = 18  # Magic default
    if d['age'] < 0: errors.append("Bad age")  # Global state
    # ... validation mixed with normalization mixed with defaults
    return d if not errors else None
```

## Transformation Path

1. **Separate concerns** (🌀→🧶): Extract validation, normalization, defaults
2. **Create pipeline** (🧶→🪢): Chain pure functions
3. **Add boundaries** (🌊→💧): Wrap in try-catch with specific errors
