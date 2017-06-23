# Prepare for a request

Because we need to know which item we want to show in the details of the interface, so the logic tells us need to send a weather forecast `id` to the details interface. So `domain model` needs a new `id` attribute:

```kotlin
data class Forecast(val id: Long, val date: Long, val description: String,
    val high: Int, val low: Int, val iconUrl: String)
```

`ForecastProvider` also requires a new function that returns the result after the request with `id`. `DetailActivity` will need to receive the request via the `id` request to get the weather forecast data. Since all requests will iterate over all the data sources and return the first non-null result, we can extract and define a new function:

```kotlin
private fun <T : Any> requestToSources(f: (ForecastDataSource) -> T?): T
        = sources.firstResult { f(it) }
```

This function uses a non-null type as a paradigm. It will receive a function and return a nullable object. Where the received function receives a `ForecastDataSource` and returns an nullable object. We can rewrite the last request and write a new one as follows:

```kotlin
fun requestByZipCode(zipCode: Long, days: Int): ForecastList = requestToSources {
	val res = it.requestForecastByZipCode(zipCode, todayTimeSpan())
	if (res != null && res.size() >= days) res else null
}

fun requestForecast(id: Long): Forecast = requestToSources {
	it.requestDayForecast(id)
}
```

Now the data source needs to implement a new function:

```kotlin
fun requestDayForecast(id: Long): Forecast?
```

`ForcastDb` will always get the desired value in the last request to be cached, so we can get it in this way:

```kotlin
override fun requestDayForecast(id: Long): Forecast? = forecastDbHelper.use {
    val forecast = select(DayForecastTable.NAME).byId(id).
            parseOpt { DayForecast(HashMap(it)) }
    if (forecast != null) dataMapper.convertDayToDomain(forecast) else null
}
```

`Select` from the query is very similar to the previous one. I created another tool function called `byId` because requests from` id` are very generic, and using a function like this simplifies the process and is more readable. The realization of the function is also quite simple:

```kotlin
fun SelectQueryBuilder.byId(id: Long): SelectQueryBuilder
        = whereSimple("_id = ?", id.toString())
```

It just uses the `whereSimple` function to use the `_id` field to query the data. This function is quite common, but as you can see, you can create the required extension functions based on the needs of your database structure, which can greatly simplify the readability of your code. `DataMapper` has some minor changes that are not worth mentioning. You can view them through the code base.

On the other hand, `ForecastServer` will no longer be used because the information will always be cached in the database. We can in some strange scenes to achieve some code protection, but we do not do any treatment in this example, so if it happens will only throw an exception:

```kotlin
override fun requestDayForecast(id: Long): Forecast?
        = throw UnsupportedOperationException()
```

> `Try` and` throw` are expressions
>> In Kotlin, almost everything is an expression, that is to say everything will return a value. This is very important in functional programming, when you use `try-catch` to deal with boundary problems or when throwing an exception. For example, in the previous example, we can assign an exception to the result, even if they are not the same type, rather than having to create a complete code block. It is also useful when we need to throw an exception in a `when` branch:
>>```kotlin
	val x = when(y){ 
				in 0..10 -> 1
                in 11..20 -> 2
                else -> throw Exception("Invalid")
	}
>>```
>> `Try-catch` is the same, we can according to the results of try to assign a value:
>> ```kotlin
	val x = try{ doSomething() }catch{ null }
>> ```

The last thing we need to do is create a command in the new activity to execute the request:

```kotlin
class RequestDayForecastCommand(
        val id: Long,
        val forecastProvider: ForecastProvider = ForecastProvider()) :
        Command<Forecast> {
    override fun execute() = forecastProvider.requestForecast(id)
}
```

The request returns a result of the `Forecast` 'that will be used for the activity drawing UI.
