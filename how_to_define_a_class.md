# How to define a class

If you want to declare a class, you just need to use the keyword `class` :.
```kotlin
class MainActivity{

}
```

Classes have a unique default constructor. We’ll see that we can create extra constructors for some
exceptional cases, but keep in mind that most situations only require a single constructor. Parameters
are written just after the name. Brackets are not required if the class doesn’t have any content:

```kotlin
class Person(name: String, surname: String)
```

Where’s the body of the constructor then? You can declare an  `init` block:
```kotlin
class Person(name: String, surname: String) {
    init{
        ...
    }
}
```
