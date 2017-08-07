# Class inheritance

By default, a class always extends from  `Any` (similar to` Object` in java), but we can extend any other
classes. Classes are closed by default (final), so we can only extend a class if it’s explicitly declared as `open` or` abstract`:

```kotlin
open class Animal(name: String)
class Person(name: String, surname: String) : Animal(name)
```

Note that when using the single constructor nomenclature, we need to specify the parameters we’re
using for the parent constructor. That’s the equivalent to  `super` call in Java.
