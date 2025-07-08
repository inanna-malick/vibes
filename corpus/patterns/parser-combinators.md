# Pattern: Parser Combinators

## VIBES Rating
`<🔬🪢💠>`

## Description
Parser combinators achieve the rare feat of being both general-purpose and constraint-perfect. They make invalid parsers impossible to construct while providing infinite compositional flexibility.

## Example
```haskell
-- Parser combinators with crystalline clarity
data Parser a = Parser (String -> Maybe (a, String))

-- Basic combinators
char :: Char -> Parser Char
char c = Parser $ \input -> case input of
  (x:xs) | x == c -> Just (c, xs)
  _               -> Nothing

-- Compositional magic
string :: String -> Parser String
string = traverse char

-- Type-safe JSON parser
jsonValue :: Parser JSON
jsonValue = jsonNull 
        <|> jsonBool 
        <|> jsonNumber 
        <|> jsonString 
        <|> jsonArray 
        <|> jsonObject
  where
    jsonNull = string "null" $> Null
    jsonBool = (string "true" $> Bool True) 
           <|> (string "false" $> Bool False)
    -- ... other parsers compose similarly
```

## Why This Rating

### Expressive Power: 🔬
- Infinite valid compositions from simple primitives
- Every composition maintains parse safety
- Multiple ways to express same parser, all correct
- General framework that constrains perfectly

### Context Flow: 🪢
- Strictly linear parsing pipeline
- Each combinator depends only on its input
- No hidden state or backtracking by default
- Composition is mathematical function composition

### Error Surface: 💠
- Invalid parsers cannot be constructed
- Type system prevents parsing impossibilities
- All failures handled through Maybe/Either
- Parse errors contain exact position and expectation

## Common Occurrence
- Haskell parsing libraries (Parsec, Megaparsec)
- Scala parser combinators
- Rust's nom (approaching this ideal)
- Academic language implementations

## Impact
- LLMs can safely compose parsers without fear
- Clear patterns enable accurate generation
- Type guidance prevents invalid combinations
- Debugging shows exact parse failure point

## Related Patterns
- [typed-schema.md](./typed-schema.md) - Similar type-driven safety
- [promise-pipeline.md](./promise-pipeline.md) - Similar compositional flow
- [module-pattern.md](./module-pattern.md) - Similar independence

## Consensus Data
- Model Agreement: 88%
- Disputed Aspects: Some models rate 🔍 for expressive due to learning curve
- Test Date: July 2025
