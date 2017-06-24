# Production operator

#### merge

The two sets are merged into a new, identical index element by a given function to merge into a new element as an element of the new collection, returning the new collection. The size of the new collection is determined by the size of the smallest set.

```kotlin
val list = listOf(1, 2, 3, 4, 5, 6)
val listRepeated = listOf(2, 2, 3, 4, 5, 5, 6)
assertEquals(listOf(3, 4, 6, 8, 10, 11), list.merge(listRepeated) { it1, it2 -> it1 + it2 })
```

#### partition

Divide a given set into two, the first set is composed of elements of the original set of elements that match the given function condition to return `true`, and the second set is matched by the original set of each element Set the function condition to return the `false` 's element.

```kotlin
assertEquals(
	Pair(listOf(2, 4, 6), listOf(1, 3, 5)), 
	list.partition { it % 2 == 0 }
)
```

#### plus

Returns a collection containing all the elements in the original collection and a given set. Because of the name of the function, we can use the `+` operator.

```kotlin
assertEquals(
	listOf(1, 2, 3, 4, 5, 6, 7, 8), 
	list + listOf(7, 8)
)
```

#### zip

Returns a List consisting of `pair`, each` pair` consists of the same index elements in both collections. The size of the returned List is determined by the smallest set.

```kotin
assertEquals(
	listOf(Pair(1, 7), Pair(2, 8)), 
	list.zip(listOf(7, 8))
)
```

#### unzip

Generates a Pair containing a List from the List containing the pair.

```kotlin
assertEquals(
	Pair(listOf(5, 6), listOf(7, 8)), 
	listOf(Pair(5, 7), Pair(6, 8)).unzip()
)
```
