# Ranges

It is difficult to explain `control flow`, if not talk about skipping. But their scope is much wider. `Range` expression uses a`..` operator, which is defined to implement a `RangTo` method.

`Ranges` helps us to use a lot of creative ways to simplify our code. For example, we can put it:

```kotlin
if(i >= 0 && i <= 10) 
	println(i)
```

转化成：

```kotlin
if (i in 0..10)
    println(i)
```

`Range` is defined as any type that can be compared, but for numeric types, the comparator optimizes it by converting it to a simple Java code to avoid extra overhead. Numeric `scope`s can also be iterated, and the compiler will convert them to the same bytecode as the for loop of index in Java:

```kotlin
for (i in 0..10)
    println(i)
```

`Ranges` will grow by default, so if the following code is used:

```kotlin
for (i in 10..0)
    println(i)
```

It will not do anything. But you can use the `downTo` function:

```kotlin
for(i in 10 downTo 0)
	println(i)
```

We can use `step` in` range` to define a different gap from 1 to a value:

```kotlin
for (i in 1..4 step 2) println(i)

for (i in 4 downTo 1 step 2) println(i)
```

If you want to create an open range (you do not include the last term, you can use the `until` function:

```kotlin
for (i in 0 until 4) println(i)
```

This line will print from 0 to 3, but will skip the last value. That is to say `0 until 4 == 0..3`. When iterating over a list, it is easier to understand `(i in 0 until list.size)` than `(i in 0..list.size - 1) `.

As mentioned before, the use of `ranges` does have a creative way. For example, a simple way to get a Views list from a `ViewGroup` can do this:

```kotlin
val views = (0..viewGroup.childCount - 1).map { viewGroup.getChildAt(it) }
```

`ranges` mix function and operator we can avoid the use of loop iterations to a set of clear, there is definitely going to add that we create a list views are. Everything is done in a line of code.

If you want to know more about the implementation of `ranges` and more examples and swimming information, you can go to [Kotlin reference]

[Kotlin reference]:  https://kotlinlang.org/docs/reference/ranges.html
