# Commissioned

[Delegate mode] is a useful model that can be used to extract the main part of the class from the class. The delegate pattern is native to Kotlin, so it avoids the need to call delegates. The delegate only needs to specify an instance of the implemented interface.

In our previous example, we can use the constructor to specify how the animal is flying, rather than implementing it. For example, an animal flying with wings can be specified in this way:

```kotlin
interface CanFly {
	fun fly()
}

class Bird(f: CanFly) : CanFly by f
```

We can use the interface to indicate that the bird can fly, but the bird's flight mode is defined in a delegate, which is defined in the constructor, so we can use different modes of flight for different birds. Animals use wings to fly in the other class:

```kotlin
class AnimalWithWings : CanFly {
    val wings: Wings = Wings()
    override fun fly() = wings.move()
}
```

Animals flap wings to fly. So we can create a bird that uses wings to fly:

```kotlin
val birdWithWings = Bird(AnimalWithWings())
birdWithWings.fly()
```

But now the wings can be used by other animals that are not birds. If we assume that bats use wings, we can specify the delegate directly to instantiate the object:

```kotlin
class Bat : CanFly by AnimalWithWings()
...
val bat = Bat()
bat.fly()
```

[Delegate mode]: https://en.wikipedia.org/wiki/Delegation_pattern
