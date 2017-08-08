# variables

Variables in Kotlin can be easily defined as mutable (`var`) or immutable (`val`). The idea is very similar
to using `final` in Java variables. But __immutable__ is a very important concept in Kotlin (and many
other modern languages).

An immutable object is an object whose state cannot change after instantiation. If you need a
modified version of the object, a new object needs to be created. This makes programming much
more robust and predictable. In Java, most objects are mutable, which means that any part of the
code which has access to the object can modify it, affecting the rest of the application.

Immutable objects are also thread-safe by definition. As they can’t change, no special access control
must be defined, because all threads will always get the same object.

So the way we think about coding changes a bit in Kotlin if we want to make use of immutability.
__The key concept: just use `val` as much as possible__. There will be situations (specially in Android,
where we don’t have access to the constructor of many classes) where it won’t be possible, but it
will most of the time.

Another thing mentioned before is that we usually don’t need to specify object types, they will be
inferred from the value, which makes the code cleaner and faster to modify. We already have some
examples from the section above.

```kotlin
val s = "Example" // A String
val i = 23 // An Int
val actionBar = supportActionBar // An ActionBar in an Activity context
```

However, a type needs to be specified if we want to use a more generic type:

```kotlin
val a: Any = 23
val c: Context = activity
```
