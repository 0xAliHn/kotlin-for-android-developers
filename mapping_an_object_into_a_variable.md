# Mapping an object into a variable

This process is known as **multi-declaration** and consists of mapping each property inside an object
into a variable. That’s the reason why the `componentX` functions are automatically created. An
example with the previous `Forecast` class:

```kotlin
val f1 = Forecast(Date(), 27.5f, "Shiny day")
val (date, temperature, details) = f1
```

This multi-declaration is compiled down to the following code:

```kotlin
val date = f1.component1()
val temperature = f1.component2()
val details = f1.component3()
```

The logic behind this feature is very powerful, and can help simplify the code in many situations.
For instance, `Map` class has some extension functions implemented that allow to recover its keys and
values in an iteration:

```kotlin
for ((key, value) in map) {
	Log.d("map", "key:$key, value:$value")
}
```
