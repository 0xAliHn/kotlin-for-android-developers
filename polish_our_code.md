# Polish our code

We are already ready to use `public` for refactoring, but we have a lot of other details that need to be modified. For example, in `RequestForecastCommand`, we create the property `zipCode` in the constructor that can be defined as `private`:

```kotlin
class RequestForecastCommand(private val zipCode: String)
```

What we have done is that we create an unmodifiable attribute zipCode whose value we can only get and can not modify it. So this little change makes the code look clearer. If we are in the preparation of the class, you think some attributes because of what reason can not be visible to others, then it is defined as `private`.

And, in Kotlin, we do not need to specify the return type of a function, which allows the compiler to infer it. Give an example of the return value type:

```kotlin
data class ForecastList(...) {
	fun get(position: Int) = dailyForecast[position]
	fun size() = dailyForecast.size()
}
```

We can omit the return value type of the typical scenario is when we want to give a function or an attribute assignment time. Without having to write code blocks to achieve.

The rest of the changes are fairly simple, and you can synchronize them in the code base.
