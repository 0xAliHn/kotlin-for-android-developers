# Delegate attribute

We may need a property with some of the same behavior, using `lazy` or` observable` can be very interesting to achieve reuse. Instead of declaring the same code again and again, Kotlin provides a way to delegate attributes to a class. This is what we know about the `delegate attribute`

When we use the `get` or `set` attribute, the `getValue` and `setValue` 'of the delegate are called.

The structure of the attribute delegate is as follows:

```kotlin
class Delegate<T> : ReadWriteProperty<Any?, T> {
	fun getValue(thisRef: Any?, property: KProperty<*>): T {
		return ...
	}
	
	fun setValue(thisRef: Any?, property: KProperty<*>, value: T) {
		...
	}
}
```

This is the type of delegate attribute. The `getValue` function takes a reference to a class and a property's metadata. The `setValue` function in turn receives a set value. If this property is not modifiable (val), there will be only one `getValue` function.

The following shows how the property is set up:

```kotlin
class Example {
	var p: String by Delegate()
}
```

It uses the `by` keyword to specify a delegate object.
