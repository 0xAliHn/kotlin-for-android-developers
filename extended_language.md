# Extended language

Thanks to these changes, we can go to create your own `builder` and code blocks. We are already using some interesting functions, such as `with`. The following simple implementation:

```kotlin
inline fun <T> with(t: T, body: T.() -> Unit) { t.body() }
```

This function takes a `T` type object and a function that is used as an extension function. Its implementation just let this object execute this function. Because the second argument is a function, so we can put it outside the parentheses, so we can create a block of code in which we can use `this` and direct access to all public methods and properties :

```kotlin
with(forecast) {
	Picasso.with(itemView.ctx).load(iconUrl).into(iconView)
	dateView.text = date
	descriptionView.text = description
	maxTemperatureView.text = "$high"
	minTemperatureView.text = "$low"
	itemView.setOnClickListener { itemClick(this) }
}
```

> Inline function
>> Inline functions are a bit different from ordinary functions. An inline function is replaced at compile time, not a real method call. This in some cases can reduce memory allocation and run-time overhead. For example, if we have a function, we only accept a function as its argument. If it is an ordinary function, the internal will create an object that contains the function. On the other hand, the inline function will replace the place where we call this function, so it does not need to generate an internal object for it.

Another example: we can create code blocks that only provide `Lollipop` or later to execute:

```kotlin
inline fun supportsLollipop(code: () -> Unit) {
	if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
		code()
	}
}
```

It just checks the version, and then if the conditions are met. Now we can do this:

```kotlin
supportsLollipop {
	window.setStatusBarColor(Color.BLACK)
}
```

For example, Anko is based on this idea to achieve `DSL` of the `Android Layout`. You can also see an example of  `Kotlin reference` [`use of DSL to write HTML`] .

[`use of DSL to write HTML`]: http://kotlinlang.org/docs/reference/type-safe-builders.html
