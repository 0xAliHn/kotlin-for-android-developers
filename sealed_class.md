# Sealed class

The sealed class is used to restrict the inheritance of classes, which means that the number of subclasses of the sealed class is fixed. It looks like an enumeration, and when you want to find a specified class in a subclass of a sealed class, you can know all the subclasses in advance. The difference is that the enumerated instance is unique, and the sealed class can have many instances, and they can have different states.

We can achieve, for example, similar to Scala in the `Option` class: this type can prevent the use of null, when the object contains a value to return `Some` class, when the object is empty,

```kotlin
sealed class Option<out T> {
    class Some<out T> : Option<T>()
    object None : Option<Nothing>()
}
```

There is a very good thing about the sealed class is that when we use the `when` expression, we can match all the options without using the `else` branch:

```kotlin
val result = when (option) {
    is Option.Some<*> -> "Contains a value"
    is Option.None -> "Empty"
}
```


