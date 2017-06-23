# Order operator

#### reverse

Returns a list in the reverse order of the specified list.

```kotlin
val unsortedList = listOf(3, 2, 7, 5)
assertEquals(listOf(5, 7, 2, 3), unsortedList.reverse())
```

#### sort

Returns a list of naturally sorted ones.

```kotlin
assertEquals(listOf(2, 3, 5, 7), unsortedList.sort())
```

#### sortBy

Returns a list sorted by the specified function.

```kotlin
assertEquals(listOf(3, 7, 2, 5), unsortedList.sortBy { it % 3 })
```

#### sortDescending

Returns a descending sorted list.

```kotlin
assertEquals(listOf(7, 5, 3, 2), unsortedList.sortDescending())
```

#### sortDescendingBy

Returns a list that is sorted by descending sorted by the specified function.

```kotlin
assertEquals(listOf(2, 5, 7, 3), unsortedList.sortDescendingBy { it % 3 })
```
