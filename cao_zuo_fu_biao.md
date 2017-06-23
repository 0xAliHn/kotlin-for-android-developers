# Operator table

Here you can see a series of tables that include the `operator` and `corresponding methods`. The corresponding method must be implemented in the specified class by various possibilities.


__unary operator__

| Operators | Functions |
|----|
| +a | a.unaryPlus() |
| -a | a.unaryMinus() |
| !a | a.not() |
| a++ | a.inc() |
| a-- | a.dec() |
<br/>
__binary operator__

| Operators | Functions |
|----|
| a + b | a.plus(b) |
| a - b | a.minus(b) |
| a * b | a.times(b) |
| a / b | a.div(b) |
| a % b | a.mod(b) |
| a..b | a.rangeTo(b) |
| a in b | a.contains(b) |
| a !in b | !a.contains(b) |
| a += b | a.plusAssign(b) |
| a -= b | a.minusAssign(b) |
| a *= b | a.timesAssign(b) |
| a /= b | a.divAssign(b) |
| a %= b | a.modAssign(b) |
<br/>
__array operator__

| Operators | Functions |
|----|
| a[i] | a.get(i) |
| a[i, j] | a.get(i, j) |
| a[i\_1, ..., i\_n] | a.get(i\_1, ..., i\_n) |
| a[i] = b | a.set(i, b) |
| a[i, j] = b | a.set(i, j, b) |
| a[i\_1, ..., i\_n] = b | a.set(i\_1, ..., i\_n, b) |
<br/>
__is equal to operator__

| Operators | Functions |
|----|
| a == b | a?.equals(b) ?: b === null |
| a != b | !(a?.equals(b) ?: b === null) |

Equal operators are a bit different, in order to achieve the correct and appropriate checks to do a more complex conversion, because to get an exact function structure comparison, not just the specified name. The method must be implemented as follows:

```kotlin
operator fun equals(other: Any?): Boolean
```
The operators `===` and `!==` are used for identity checking (they are `==` and `!=` In Java, respectively), and they can not be overloaded.

<br/>
__function call__

| Method | call |
|----|
| a(i) | a.invoke(i) |
| a(i, j) | a.invoke(i, j) |
| a(i\_1, ..., i\_n)| a.invoke(i\_1, ..., i\_n) |
