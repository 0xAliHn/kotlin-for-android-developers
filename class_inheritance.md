# Class inheritance

By default any class is based on `Any` (similar to` Object` in java), but we can inherit other classes. All classes are not inherited by default (final), so we can only inherit those explicitly stated `open` or` abstract` categories:

```kotlin
open class Animal(name: String)
class Person(name: String, surname: String) : Animal(name)
```

When we have only a single constructor, we need to specify the required parameters in the constructor inherited from the parent class. This is used to replace the `super` call in Java.
