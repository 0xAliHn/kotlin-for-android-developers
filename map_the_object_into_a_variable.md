# Map the object into a variable

To map each object of a property to a variable, this process is the multi-declaration we know. That's why the `componentX` function is created automatically. Using the above example `Forecast` class:

```kotlin
val f1 = Forecast(Date(), 27.5f, "Shiny day")
val (date, temperature, details) = f1
```

The above statement will be compiled into the following code:

```kotlin
val date = f1.component1()
val temperature = f1.component2()
val details = f1.component3()
```

The logic behind this feature is very powerful, it can in many cases help us to simplify the code. For example, the `Map` class contains implementations of some extended functions that allow it to use key and value in iterations:

```kotlin
for ((key, value) in map) {
	Log.d("map", "key:$key, value:$value")
}
```
