# Total operator

#### any

Returns true if at least one element matches the given judgment condition.

```kotlin
val list = listOf(1, 2, 3, 4, 5, 6)
assertTrue(list.any { it % 2 == 0 })
assertFalse(list.any { it > 10 })
```

#### all

Returns true if all elements match the given judgment condition.

```kotlin
assertTrue(list.all { it < 10 })
assertFalse(list.all { it % 2 == 0 })
```

#### count

Returns the total number of elements that match the criteria given.

```kotlin
assertEquals(3, list.count { it % 2 == 0 })
```

#### fold

Accumulates all the elements from the first item to the last item on the basis of an initial value.

```kotlin
assertEquals(25, list.fold(4) { total, next -> total + next })
```

#### foldRight

Same as `fold`, but the order is from the last item to the first item.

```kotlin
assertEquals(25, list.foldRight(4) { total, next -> total + next })
```

#### forEach

Traverse all the elements and perform the given operation.

```kotlin
list.forEach { println(it) }
```

#### forEachIndexed

And `forEach`, but we can get the index of the element at the same time.

```kotlin
list.forEachIndexed { index, value
		-> println("position $index contains a $value") }
```

#### max

Returns the largest item, or null if no.

```kotlin
assertEquals(6, list.max())
```

#### maxBy

Returns the largest item according to the given function, or null if it does not.

```kotlin
// The element whose negative is greater
assertEquals(1, list.maxBy { -it })
```

#### min

Returns the smallest item, or null if none.

```kotlin
assertEquals(1, list.min())
```

#### minBy

Returns the smallest item based on the given function, or null if it does not.

```kotlin
// The element whose negative is smaller
assertEquals(6, list.minBy { -it })
```

#### none

Returns true if no element matches a given function.

```kotlin
// No elements are divisible by 7
assertTrue(list.none { it % 7 == 0 })
```

#### reduce

Same as `fold`, but not an initial value. Through a function from the first item to the last one to accumulate.

```kotlin
assertEquals(21, list.reduce { total, next -> total + next })
```

#### reduceRight

Same as `reduce`, but the order is from the last item to the first item.

```kotlin
assertEquals(21, list.reduceRight { total, next -> total + next })
```

#### sumBy

Returns the sum of all the data after each function conversion.

```kotlin
assertEquals(3, list.sumBy { it % 2 })
```
