# basic type

Of course, types like integer, float, or boolean still exist, but they all exist as objects. Basic types of names and how they work are very similar to Java, but there are some differences you might need to consider:

- The number type does not automatically transition. For example, you can not assign a variable to `Double` or `Int`. Must do a clear type conversion, you can use one of the many functions:

```kotlin
val i:Int=7
val d: Double = i.toDouble()
```

- The character (Char) can not be treated directly as a number. We need to convert them into a number when needed:

```kotlin
val c:Char='c'
val i: Int = c.toInt()
```

- Bit operations are also a bit different. In Android, we often use "or" in `flags`, so I use "and" and "or"  for example:

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

>There are many other bitwise operators, such as `sh1`, `shs`, `ushr`, `xor` or `inv`. When we need time, you can [Kotlin official website] view.

- Literally can specify the specific type. This is not necessary, but a generic Kotlin practice omit the type of the variable (we can see it right away), so we can let the compiler themselves deduce the specific type.

```kotlin
val i = 12 // An Int
val iHex = 0x0f // 一Hex type of hex
val l = 3L // A Long
val d = 3.5 // A Double
val f = 3.5F // A Float
```

- 一A String can be accessed like an array and iterated:

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
