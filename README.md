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
if (can-find(first order where order.orderdate < today - 10 and lookup(order.orderstatus, 'open,hold,busy' ) > 0) then
    // ...
```

but:
```
define variable openOrders as logical no-undo.

openOrders = (can-find(first order where order.orderdate < today - 10 and lookup(order.orderstatus, 'open,hold,busy' ) > 0).
if (openOrders) then 
    // ...
```
Use a variable to tell the reader what the expression actually represents. Use that variable for follow up behavior.
The performance impact of using a variable is negligible, the readability is increased dramatically.

## functions/methods
Try for your methods and functions to be pure. This mean that the result is a function of the input parameters AND has no side effects. The latter is not always possible (think database I/O), but well worth striving for. It makes your code predictable and testables. Try to avoid using "globals" in functions/methods. If they are needed inject then parameter. So:

```
class Foo:
  
    define private variable Y as integer no-undo.

    // anti pattern:
    method private integer add (x as integer):
        return x + this-object:Y.
    end method.

    // use:
    method private integer add (x as integer, z as integer):
        return x + z.
    end method.

    // and call add(1, Y).
end class.
```
