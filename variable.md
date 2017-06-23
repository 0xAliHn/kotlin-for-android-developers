# variable

Variables can be simply defined as variables that are variable (`var`) and immutable (`val`). This is similar to `final` used in Java. But __immutable__ is a very important concept in Kotlin (and many other modern languages).

一An immutable object means that it can not change its state after instantiation. If you need a version of this object after the modification, then it will create a new object. This makes programming more robust and predictable. In Java, most of the object is variable, it means that any code that can access it can modify it, thus affecting the rest of the program.

Invariable objects can also be thread-safe because they can not be changed, nor do they need to define access control, because all threads access the same object.

So in Kotlin, if we want to use immutable, we need to have some changes in the way we think about it. __an important concept is: use `val`__ as much as possible. In addition to individual circumstances (especially in Android, there are many classes we will not directly call the constructor), most of the time is possible.

The other thing that is mentioned earlier is that we usually do not need to specify the type of class, which is automatically inferred from the later assigned statement, which makes the code clearer and quicker to modify. We have some examples in front of us:


```kotlin
val s = "Example" // A String
val i = 23 // An Int
val actionBar = supportActionBar // An ActionBar in an Activity context
```

If we need to use more generic types, we need to specify:

```kotlin
val a: Any = 23
val c: Context = activity
```
