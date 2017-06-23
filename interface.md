# interface

Kotlin interface is much more powerful than Java 7. If you use Java 8, they are very similar. In Kotlin, we can use interfaces like Java. Imagine that we have some animals, some of them can fly. This is our interface to the flying animal:

```kotlin
interface FlyingAnimal {
	fun fly()
}
```

Birds and bats can fly by flapping wings. So we create two classes for them:

```kotlin
class Bird : FlyingAnimal {
    val wings: Wings = Wings()
    override fun fly() = wings.move()
}

class Bat : FlyingAnimal {
    val wings: Wings = Wings()
    override fun fly() = wings.move()
}
```

When two classes inherit from an interface, it is very typical that both of them share the same implementation. However, the interface in Java 7 can only define behavior, but it can not be implemented.

The Kotlin interface in one aspect it can implement the function. The only difference between them and the class is that they are stateless, so attributes require subclasses to be overridden. Classes need to be responsible for saving the status of interface properties.

We can let the interface implement `fly` function:

```kotlin
interface FlyingAnimal {
    val wings: Wings
    fun fly() = wings.move()
}
```

As mentioned, classes need to rewrite attributes:

```kotlin
class Bird : FlyingAnimal {
	override val wings: Wings = Wings()
}

class Bat : FlyingAnimal {
	override val wings: Wings = Wings()
}
```

Now birds and bats can fly:

```kotlin
val bird = Bird()
val bat = Bat()

bird.fly()
bat.fly()
```
