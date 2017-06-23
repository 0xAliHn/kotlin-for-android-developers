# Element operator

#### contains

Returns true if the specified element can be found in the collection.

```kotlin
assertTrue(list.contains(2))
```

#### elementAt

Returns the element corresponding to the given index, which throws `IndexOutOfBoundsException` if the index array is out of bounds.

```kotlin
assertEquals(2, list.elementAt(1))
```

#### elementAtOrElse

Returns the element corresponding to the given index. If the index array is out of bounds, the default value is returned according to the given function.

```kotlin
assertEquals(20, list.elementAtOrElse(10, { 2 * it }))
```

#### elementAtOrNull

Returns the element corresponding to the given index, or null if the index array is out of bounds.

```kotlin
assertNull(list.elementAtOrNull(10))
```

#### first

Returns the first element that matches the given function's condition.

```kotlin
assertEquals(2, list.first { it % 2 == 0 })
```

#### firstOrNull

Returns the first element that matches the given function's condition, or null if there is no match.

```kotlin
assertNull(list.firstOrNull { it % 7 == 0 })
```

#### indexOf

Returns the first index of the specified element, or `-1` if it does not exist.

```kotlin
assertEquals(3, list.indexOf(4))
```

#### indexOfFirst

Returns the index of the first element that matches the condition of the given function, or `-1` if it does not match.

```kotlin
assertEquals(1, list.indexOfFirst { it % 2 == 0 })
```

#### indexOfLast

Returns the index of the last element that matches the condition of the given function, and returns `-1` if it does not match.

```kotlin
assertEquals(5, list.indexOfLast { it % 2 == 0 })
```

#### last

Returns the last element that matches the given function condition.

```kotlin
assertEquals(6, list.last { it % 2 == 0 })
```

#### lastIndexOf

Returns the last index of the specified element, or `-1` if it does not exist.

#### lastOrNull

Returns the last element that matches the given function condition, or null if there is no match.

```kotlin
val list = listOf(1, 2, 3, 4, 5, 6)
assertNull(list.lastOrNull { it % 7 == 0 })
```

#### single

Returns a single element that matches the given function, and throws an exception if it does not match or exceeds one.

```kotlin
assertEquals(5, list.single { it % 5 == 0 })
```

#### singleOrNull

Returns a single element that matches the given function, or null if none or more than one.

```kotlin
assertNull(list.singleOrNull { it % 7 == 0 })
```
