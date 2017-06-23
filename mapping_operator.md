# Mapping operator

#### flatMap

Traverse all the elements, create a collection for each, and finally put all the collections in a collection.

```kotlin
assertEquals(listOf(1, 2, 2, 3, 3, 4, 4, 5, 5, 6, 6, 7), 
list.flatMap { listOf(it, it + 1) })
```

#### groupBy

Returns a map that is grouped according to a given function.

```kotlin
assertEquals(mapOf("odd" to listOf(1, 3, 5), "even" to listOf(2, 4, 6)), list.groupBy { if (it % 2 == 0) "even" else "odd" })
```

#### map

Returns a List of each element that is converted according to the given function.

```kotlin
assertEquals(listOf(2, 4, 6, 8, 10, 12), list.map { it * 2 })
```

#### mapIndexed

Returns a List of each element consisting of a given function that contains the element index.

```kotlin
assertEquals(listOf (0, 2, 6, 12, 20, 30), list.mapIndexed { index, it -> index * it })
```

#### mapNotNull

Returns a List of each non-null element that is converted according to the given function.

```kotlin
assertEquals(listOf(2, 4, 6, 8), listWithNull.mapNotNull { it * 2 })
```
