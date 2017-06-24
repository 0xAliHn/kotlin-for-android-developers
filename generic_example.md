# Generic example

After the theory, we moved to some of the actual functions above, which would make it easier for us to master it. In order not to repeat the invention of the wheel, I used three functions in the three Kotlin standard libraries. These functions let us only use the generic implementation can do some great things. It can inspire you to create your own function.

#### let

`Let` is really a simple function that can be called by any object. It takes a function (receives an object, returns the result of the function) as a parameter, and returns the result as a function of the return value of the entire function. It is very useful in dealing with null objects, the following is its definition:

```kotlin
inline fun <T, R> T.let(f: (T) -> R): R = f(this)
```

It uses two generic types: `T` and` R`. The first one is defined by the caller, and its type is received by the function. The second is the return type of the function.

How do we use it? You may remember that when we get data from the data source, the result may be null. If it is not null, the result is mapped to `domain model` and returns the result, otherwise it returns null:

```kotlin
if (forecast != null) dataMapper.convertDayToDomain(forecast) else null
```

This code is very ugly, and we do not need to use this way to handle nullable objects. In fact, if we use `let`, do not need` if`:

```kotlin
forecast?.let { dataMapper.convertDayToDomain(it) }
```

Thanks to the `?.` operator, the `let` function will only be executed if` forecast` is not null. Otherwise it will return null. That is, we want to achieve the effect.

#### with

In this book we have a lot of this function. `With` receives an object and a function that functions as an extension of the object. This means that we can use `this` in the function according to the inference.

```kotlin
inline fun <T, R> with(receiver: T, f: T.() -> R): R = receiver.f()
```

Generics are also run in the same way here: `T` represents the receive type, `R` represents the result. As you can see, the function is defined as an extension function with the `f: T.() -> R` declaration. That's why we can call `receiver.f()`.

Through this app, we have a few examples:

```kotlin
fun convertFromDomain(forecast: ForecastList) = with(forecast) {
    val daily = dailyForecast map { convertDayFromDomain(id, it) }
    CityForecast(id, city, country, daily)
}
```

#### apply

It looks like `with` very similar, but it is a bit different. `Apply` can be used to avoid creating a builder because the function called by the object can initialize itself according to its own needs, and then the `apply` function will return it to the same object:

```kotlin
inline fun <T> T.apply(f: T.() -> Unit): T { f(); return this }
```

Here we only need a generic type, because the object that calls this function is the object that this function returns. A nice example:

```kotlin
val textView = TextView(context).apply {
    text = "Hello"
    hint = "Hint"
    textColor = android.R.color.white
}
```

It creates a `TextView`, modifies some properties, and then assigns it to a variable. Everything is simple, readable and rugged. Let's use it in the current code. In `ToolbarManager`, we use this way to create a navigation drawable:

```kotlin
private fun createUpDrawable() = with(DrawerArrowDrawable(toolbar.ctx)) {
    progress = 1f
	this
}
```

Using `with` and returning` this` is very clear, but using `apply` can be easier:

```kotlin
private fun createUpDrawable() = DrawerArrowDrawable(toolbar.ctx).apply {
	progress = 1f
}
```

You can view these small optimizations in the `Kotlin for Android Developer` codebase.
