# Spreadsheet Formula Pattern

## Pattern: <👓🌀💧>

```excel
=IF(ISBLANK(A2),"",
  IF(VLOOKUP(A2,Sheet2!$A$1:$C$100,3,FALSE)>100,
    IF(AND(B2>DATE(2024,1,1),B2<TODAY()),
      SUMIF(Sheet3!$A:$A,A2,Sheet3!$C:$C)*
        (1+VLOOKUP(A2,Config!$A$1:$B$50,2,FALSE)),
      SUMIF(Sheet3!$A:$A,A2,Sheet3!$D:$D)),
    "Check manually"))
```

## VIBES Analysis

- **Expressive** (👓): One way to write formulas, rigid syntax
- **Context** (🌀): Circular references between sheets, hidden dependencies
- **Error** (💧): #REF!, #VALUE!, #N/A cascade through dependent cells

## Why This Matters

This is how millions of people program daily. Not in IDEs with type systems, but in Excel with formulas that run businesses, calculate budgets, make decisions. The VIBES framework must speak to this reality.

## The Visceral Experience

You know that moment when you open someone else's spreadsheet and see a wall of nested IFs? The dread as you try to trace which cells feed which formulas? The fear of changing anything because the whole model might break?

This is programming for most of the world.

## Real-World Impact

```excel
# Budget spreadsheet that allocates millions
=C2*IF(MONTH(TODAY())>6,
  INDEX(Rates!$B$2:$B$13,MONTH(TODAY())),
  INDEX(Rates!$C$2:$C$13,MONTH(TODAY())))

# One wrong cell reference = entire department's budget corrupted
```

## Anti-Pattern Version

```excel
# <🙈🌀🌊>
='Q1'!B2+'Q2'!B2+'Q3'!B2+'Q4'!B2+Adjustments!B2*'Summary (2)'!$C$2

# Sheets renamed? Formula breaks.
# Row inserted? References shift.
# Someone deletes 'Summary (2)'? #REF! everywhere.
```

## Better Pattern

```excel
# <🔍🪢🧊>
# Named ranges with validation
=SUM(QuarterlySales)*TaxRate

# Structured references in tables
=[@[Unit Price]]*[@Quantity]*(1-[@Discount])

# Error handling built in
=IFERROR(XLOOKUP([@Product],Products[SKU],Products[Price]),"Price not found")
```

## Transformation Path

1. **Name your ranges** (🙈→👓): No more cryptic cell references
2. **Use tables** (🌀→🧶): Structured references survive changes
3. **Add validation** (🌊→💧): Data validation prevents bad inputs
4. **Document inline** (👓→🔍): N() function for formula comments

## Cultural Bridge

For programmers: Imagine every variable is a global mutable. No functions, only macros. No debugger, only #ERROR!. This is the daily reality of spreadsheet programming.

For spreadsheet users: Those cryptic formulas are code. VIBES helps make that code less fragile, more understandable, more maintainable.

## Why Include This

VIBES claims to evaluate "expression languages for LLM use." Millions use LLMs to write Excel formulas daily. This pattern makes VIBES relevant beyond traditional programming, acknowledging computation where it actually happens.
