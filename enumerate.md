# enumerate

Kotlin also provides an enumeration (`enums`) implementation:

```kotlin
enum class Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY,
    THURSDAY, FRIDAY, SATURDAY
}
```

Enumerations can have parameters:

```kotlin
enum class Icon(val res: Int) {
	UP(R.drawable.ic_up),
	SEARCH(R.drawable.ic_search),
	CAST(R.drawable.ic_cast)
}

val searchIconRes = Icon.SEARCH.res
```

Enumeration can be obtained through `String` matching name, we can get that contains all enumerated` Array`, so we can traverse it.

```kotlin
val search: Icon = Icon.valueOf("SEARCH")
val iconList: Array<Icon> = Icon.values()
```

And each enumeration has some function to get its name, declared the location:

```kotlin
val searchName: String = Icon.SEARCH.name()
val searchPosition: Int = Icon.SEARCH.ordinal()
```

Enumerate the `Comparable` interface according to its order, so it's easy to sort them.
