# How to create a custom delegate

Let's say what we want to achieve, for example, we create a `notNull` delegate, it can only be assigned once, if the second assignment, it will throw exception.

The Kotlin library provides several interfaces, and our own delegates must implement: `ReadOnlyProperty` and `ReadWriteProperty` . Depending on whether the object we are entrusted is `val` or `var`.

The first thing we have to do is create a class and then inherit `ReadWriteProperty`:

```kotlin
private class NotNullSingleValueVar<T>() : ReadWriteProperty<Any?, T> {

		override fun getValue(thisRef: Any?, property: KProperty<*>): T {
	        throw UnsupportedOperationException()
        }
           
		override fun setValue(thisRef: Any?, property: KProperty<*>, value: T) {
		}
}
```

This delegate can be applied to any non-null type. It receives any type of reference, and then uses the same as getter and setter. Now we need to implement these functions.

- If the Getter function has been initialized, it will return a value, otherwise it will throw an exception.
- Setter function if it is still null, then assignment, otherwise it will throw an exception.

```kotlin
private class NotNullSingleValueVar<T>() : ReadWriteProperty<Any?, T> {
    private var value: T? = null
    override fun getValue(thisRef: Any?, property: KProperty<*>): T {
        return value ?: throw IllegalStateException("${desc.name} " +
                "not initialized")
	}
	
	override fun setValue(thisRef: Any?, property: KProperty<*>, value: T) {
		this.value = if (this.value == null) value
		else throw IllegalStateException("${desc.name} already initialized")
	}
}
```

Now you can create an object and then add the function using your delegate:

```kotlin
object DelegatesExt {
    fun notNullSingleValue<T>():
            ReadWriteProperty<Any?, T> = NotNullSingleValueVar()
}
```
