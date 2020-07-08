# 4GL coding standards

## casing
- 4GL statements: *lowercase* 
- directory names: lowercase (so also for package of `using`)
- Class names: PascalCase
- public, protected members: PascalCase
- private members: camelCase

## indentation
Indentation is 4 spaces. Not 2, not tabs.

## prefixes
No prefixes for variables, parameters for either their datatype and they're being input/output. If one thinks prefixes are necessary they arguably make their (internal) procedure and/or methods too big.

### buffers 
Buffers are prefixed by `b-`

### temp-tables 
Temp-tables are prefixed with `tt`

### datasets 
Datasets are prefixed with `ds`

### interfaces
Interfaces are prefixed with `I`

## operators
Used `=, <>, <, <=, >, >=` NOT `eq, ne, etc`
When in doubt, use parentheses for the expression:
```
success = (returnCode = 'OK')
```
(remember, operators line `+=` are on their way > 12.2)

## if-then-else
Parentheses are put around the expression.
```
if (expr) then
    // something
else
    // something different
```

and

```
if (expr) then do:
    // something
end.
else do:
    // something different
end.
```

## compound expressions
Try to make them clear, use a variable telling what the (way too long) expression is:

NOT:
```
if (can-find(first order where order.orderdate < today - 10 and lookpu(order.orderstatus, 'open,hold,busy' ) > 0) then
    // ...
```

but:
```
define variable openOrders as logical no-undo.

openOrders = (can-find(first order where order.orderdate < today - 10 and lookpu(order.orderstatus, 'open,hold,busy' ) > 0).
if (openOrders) then 
    // ...
```
Use a variable to tell the reader what the expression actually represents. Use that variable for follow up behavior.
The performance impact of using a variable is negligible, the readability is increased dramatically.
