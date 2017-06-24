# Internal class

In Java, we can redefine classes in the class. If it is a normal class, it can not access members of an external class (like static in Java):

```kotlin
class Outer {
  private val bar: Int = 1
	class Nested {
		fun foo() = 2
	}
}

val demo = Outer.Nested().foo() // == 2
```

If you need to access the members of the external class, we need to declare this class with `inner`:

```kotlin
class Outer {
	private val bar: Int = 1
	inner class Inner{
		fun foo() = bar
	}
}

val demo = Outer().Inner().foo() // == 1
```
