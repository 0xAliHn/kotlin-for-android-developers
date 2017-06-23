# Filter operator

#### drop

Returns a list containing all the elements that removed the first n elements.

```kotlin
assertEquals(listOf(5, 6), list.drop(4))
```

#### dropWhile

Returns a list of the specified elements removed from the first item according to the given function.

```kotin
assertEquals(listOf(3, 4, 5, 6), list.dropWhile { it < 3 })
```

#### dropLastWhile

Returns a list of the specified elements removed from the last item based on the given function.

```kotlin
assertEquals(listOf(1, 2, 3, 4), list.dropLastWhile { it > 4 })
```

#### filter

Filter all elements that meet the given function's condition.

```kotin
assertEquals(listOf(2, 4, 6), list .ilter { it % 2 == 0 })
```

#### filterNot

Filter all elements that do not meet the given function's condition.

```kotin
assertEquals(listOf(1, 3, 5), list.filterNot { it % 2 == 0 })
```

#### filterNotNull

Filter all elements that are not null in all elements.

```kotlin
assertEquals(listOf(1, 2, 3, 4), listWithNull.filterNotNull())
```

#### slice

Filter the elements of the specified index in the list.

```kotlin
assertEquals(listOf(2, 4, 5), list.slice(listOf(1, 3, 4)))
```

#### take

Returns the n elements starting from the first.

```kotlin
assertEquals(listOf(1, 2), list.take(2))
```

#### takeLast

Returns the n elements starting from the last one

```kotlin
assertEquals(listOf(5, 6), list.takeLast(2))
```

#### takeWhile

Returns the element that matches the given function condition from the first.

```kotlin
assertEquals(listOf(1, 2), list.takeWhile { it < 3 })
```
