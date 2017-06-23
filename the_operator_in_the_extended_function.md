# The operator in the extended function

We do not need to extend our own classes, but I need to use the extension function to extend the classes we already exist to allow third-party libraries to provide more operations. A few examples, we can go to visit the List as a way to visit the `viewGroup` view:

```kotlin
operator fun ViewGroup.get(position: Int): View = getChildAt(position)
```

Now really can be very simple from a `ViewGroup` through the position to get a view:

```kotlin
val container: ViewGroup = find(R.id.container)
val view = container[2]
```

Do not forget to go to the [Kotlin for Android developers repository] to view the code.

[Kotlin for Android developers repository]:  https://github.com/antoniolg/Kotlin-for-Android-Developers
