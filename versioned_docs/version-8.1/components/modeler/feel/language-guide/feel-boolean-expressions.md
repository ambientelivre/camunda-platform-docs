---
id: feel-boolean-expressions
title: Boolean expressions
description: "This document outlines boolean expressions and examples."
---

### Literal

Creates a new boolean value.

```feel
true

false
```

### Comparison

Two values of the same type can be compared using the following operators:

<table>
  <tr>
    <th>Operator</th>
    <th>Description</th>
    <th>Supported types</th>
  </tr>

  <tr>
    <td>=</td>
    <td>equal to</td>
    <td>any</td>
  </tr>

  <tr>
    <td>!=</td>
    <td>not equal to</td>
    <td>any</td>
  </tr>

  <tr>
    <td>&lt;</td>
    <td>less than</td>
    <td>number, date, time, date-time, duration</td>
  </tr>

  <tr>
    <td>&lt;=</td>
    <td>less than or equal to</td>
    <td>number, date, time, date-time, duration</td>
  </tr>

  <tr>
    <td>&gt;</td>
    <td>greater than</td>
    <td>number, date, time, date-time, duration</td>
  </tr>

  <tr>
    <td>&gt;=</td>
    <td>greater than or equal</td>
    <td>number, date, time, date-time, duration</td>
  </tr>

  <tr>
    <td>between [x] and [y]</td>
    <td>same as (_ &gt;= x and _ &lt;= y)</td>
    <td>number, date, time, date-time, duration</td>
  </tr>

</table>

```feel
5 = 5
// true

5 != 5
// false

date("2020-04-05") < date("2020-04-06")
// true

time("08:00:00") <= time("08:00:00")
// true

duration("P1D") > duration("P5D")
// false

duration("P1Y") >= duration("P6M")
// true

5 between 3 and 7
// true

date("2020-04-06") between date("2020-04-05") and date("2020-04-09")
// true
```

:::caution Be Careful!
The equals operator has only **one** equals sign (e.g. `x = y`). In other languages, the operator has two equals signs (e.g. `x == y`).
:::

### Null check

Any value or variable can be compared with `null` to check if it is equal to `null`, or if it exists.

Comparing `null` to a value different from `null` results in `false`. It returns `true` if the
value is `null` or the variable doesn't exist.

Comparing a context entry with `null` results in `true` if the value of the entry is `null` or if
the context doesn't contain an entry with this key.

```feel
null = null
// true

"foo" = null
// false

{x: null}.x = null
// true

{}.y = null
// true
```

:::tip

The built-in
function [is defined()](../builtin-functions/feel-built-in-functions-boolean.md#is-defined) can be
used to differentiate between a value that is `null` and a variable or context entry that doesn't
exist.

```feel
is defined(null)
// true

is defined({x: null}.x)
// true

is defined({}.y)
// false
```

:::

### Conjunction/and

Combines multiple boolean values following the ternary logic.

- The result is `true` if all values are `true`.
- The result is `false` if one value is `false`.
- Otherwise, the result is `null` (i.e. if a value is not a boolean.)

```feel
true and true
// true

true and false
// false

true and null
// null

true and "otherwise"
// null

false and null
// false

false and "otherwise"
// false
```

### Disjunction/or

Combines multiple boolean values following the ternary logic.

- The result is `true` if at least one value is `true`.
- The result is `false` if all values are `false`.
- Otherwise, the result is `null` (i.e. if a value is not a boolean.)

```feel
true or false
// true

false or false
// false

true or null
// true

true or "otherwise"
// true

false or null
// null

false or "otherwise"
// null
```

### Instance of

Checks if the value is of the given type. Available type names:

- `boolean`
- `number`
- `string`
- `date`
- `time`
- `date time`
- `day-time-duration`
- `year-month-duration`
- `list`
- `context`
- `function`
- `Any`

Use the type `Any` to check if the value is not `null`.

```feel
1 instance of number
// true

1 instance of string
// false

1 instance of Any
// true

null instance of Any
// false
```

### Unary-tests/in

Evaluates a [unary-tests](/docs/components/modeler/feel/language-guide/feel-unary-tests) with the given value. The keyword `in` separates the value from the unary-tests.

```feel
5 in (3..7)
// true

date("2021-06-04") in [date("2021-05-01")..date("2021-05-31")]
// false

5 in (3,5,7)
// true

5 in [2,4,6,8]
// false
```
