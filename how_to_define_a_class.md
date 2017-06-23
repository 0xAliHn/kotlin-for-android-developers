# How to define a class

If you want to define a class, you only need to use the `class` keyword.
```kotlin
class MainActivity{

}
```

It has a default unique constructor. We'll learn to create other extra constructors in special cases in a future lesson, but keep in mind that most of the time you only need this default constructor. You only need to write its parameters after the class name. If this class does not have any content you can omit curly braces:

```kotlin
class Person(name: String, surname: String)
```

So where is the function of the constructor? You can write in the `init` block:
```kotlin
class Person(name: String, surname: String) {
    init{
        ...
    }
}
```
