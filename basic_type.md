# basic type

Of course, basic types such as integers, f loats, characters or booleans still exist, but they all act as
an object. The name of the basic types and the way they work are very similar to Java, but there are
some differences you might take into account:

- There are no automatic conversions among numeric types. For instance, you cannot assign
an `Double` to a `Int`variable. An explicit conversion must be done, using one of the many
functions available:

```kotlin
val i:Int=7
val d: Double = i.toDouble()
```

- Characters (Char) cannot directly be used as numbers. We can, however, convert them to a
number when we need it:

```kotlin
val c:Char='c'
val i: Int = c.toInt()
```

- Bitwise arithmetical operations are a bit different. In Android, we use bitwise `or` quite often
for f lags, so I’ll stick to `and` and `or` as an example:

```java
// Java
int bitwiseOr = FLAG1 | FLAG2;
int bitwiseAnd = FLAG1 & FLAG2;
```

```kotlin
// Kotlin
val bitwiseOr = FLAG1 or FLAG2
val bitwiseAnd = FLAG1 and FLAG2
```

>There are many other bitwise operations, such as  `sh1`, `shs`, `ushr`, `xor` or `inv`.You can take
a look at the [Kotlin official website] for more information.

- Literals can give information about its type. It’s not a requirement, but a common practice in
Kotlin is to omit variable types (we’ll see it soon), so we can give some clues to the compiler
to let it infer the type from the literal:

```kotlin
val i = 12 // An Int
val iHex = 0x0f // 一Hex type of hex
val l = 3L // A Long
val d = 3.5 // A Double
val f = 3.5F // A Float
```

- A `String` can be accessed as an array and can be iterated:

```kotlin
val s = "Example"
val c = s[2] //This is a character 'a'
// Iteration String
val s = "Example"
for(c in s){
    print(c)
}
```

[Kotlin official website]: http://kotlinlang.org/docs/reference/basic-types.html#operations
