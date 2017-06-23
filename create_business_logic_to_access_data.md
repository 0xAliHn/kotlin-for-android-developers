# Create business logic to access data

After implementing the access to the server and interacting with the local database, it is time to integrate things together. The logical steps are as follows:

- Get data from the database
- Check if there is data for the corresponding week
- if yes, return to UI and render
- If not, request the server to get the data
- The result is saved in the database and returned to the UI render

But our `commands` should not handle all of these logic. Data source should be a specific implementation so that it can be easily modified, so add some extra code, then `command` abstracted from data access sounds like a good way. In our implementation, it traverses the entire list until the result is found.

So we first come to the interface to define some of our implementation `provider` need to use the data source:

```kotlin
interface ForecastDataSource {
    fun requestForecastByZipCode(zipCode: Long, date: Long): ForecastList?
}
```

`Provider` needs one to receive `zip code` and a `date`, then it should be based on that day to return the weather forecast for the week.

```kotlin
class ForecastProvider(val sources: List<ForecastDataSource> =
        ForecastProvider.SOURCES) {
        
	companion object {
	    val DAY_IN_MILLIS = 1000 * 60 * 60 * 24
	    val SOURCES = listOf(ForecastDb(), ForecastServer())
	}
    ...
}
```

`Forecast provider` receives a list of data sources, passed in the constructor (for example, for testing), but I set the source default to` SOURCES`List defined in the `companion object`. I will use the database's data source and server-side data source. The order is important because it traverses the sources according to the order, and then stops the query once a valid return value is obtained. The logical order is first in the local query (local database), and then through the API query.

So the main function of the code is as follows：

```kotlin
fun requestByZipCode(zipCode: Long, days: Int): ForecastList
            = sources.firstResult { requestSource(it, days, zipCode) }
```

It will get the first result that is not null and then return. When I searched for a large number of function operators in Chapter 18, I did not find exactly what I wanted. So when i look at the source code of Kotlin, I copied the `first` function and then modify them to achieve the purpose I want:

```kotlin
inline fun <T, R : Any> Iterable<T>.firstResult(predicate: (T) -> R?) : R {
	for (element in this){
		val result = predicate(element)
		if (result != null) return result
	}
	throw NoSuchElementException("No element matching predicate was found.")
}
```

The function receives an assert function that takes a `T` type object and then returns a value of type` R? `. This means that `predicate` can return a null type, but our` firstResult` can not return null. This is why the reason for returning `R`.

How does it work? It will traverse each element in the collection and then execute the assert function. When the result of this assertion function is not null, the result is returned.

If we can allow sources to return null, then we can use the `firstOrNull` function instead. The difference is that the last line returns null and throws exception. But I am not in the code inside to deal with these details.

In our example, `T = ForecastDataSource`,` R = ForecastList`. But remember that the function specified in `ForecastDataSource` returns a SearchList , that is, `R?`, So everything is so perfectly matched. `requestSource` makes the preceding function look more readable:

```kotlin
fun requestSource(source: ForecastDataSource, days: Int, zipCode: Long):
        ForecastList? {
    val res = source.requestForecastByZipCode(zipCode, todayTimeSpan())
    return if (res != null && res.size() >= days) res else null
}
```

If the result is not null and the number is also matched, the query is executed and only one data is returned. Otherwise, the data source does not have enough data to return a successful result.

The function `todayTimeSpan()` calculates the time of the millisecond today and excludes the "time difference". Some of the data sources (the database in our example) may need it. Because if we do not specify more information, the server default is today, so we do not need to set it.

```kotlin
private fun todayTimeSpan() = System.currentTimeMillis() / DAY_IN_MILLIS * DAY_IN_MILLIS
```

The complete code for this class is as follows:

```kotlin
class ForecastProvider(val sources: List<ForecastDataSource> =
        ForecastProvider.SOURCES) {

	companion object {
        val DAY_IN_MILLIS = 1000 * 60 * 60 * 24;
        val SOURCES = listOf(ForecastDb(), ForecastServer())
    }

	fun requestByZipCode(zipCode: Long, days: Int): ForecastList
            = sources.firstResult { requestSource(it, days, zipCode) }

	private fun requestSource(source: RepositorySource, days: Int,
            zipCode: Long): ForecastList? {
        val res = source.requestForecastByZipCode(zipCode, todayTimeSpan())
        return if (res != null && res.size() >= days) res else null
    }

	private fun todayTimeSpan() = System.currentTimeMillis() /
            DAY_IN_MILLIS * DAY_IN_MILLIS
}
```

We have defined a `ForecastDb`. Now we need to implement `ForcastDataSource`:

```kotlin
class ForecastDb(val forecastDbHelper: ForecastDbHelper =
        ForecastDbHelper.instance, val dataMapper: DbDataMapper = DbDataMapper())
        : ForecastDataSource {
        
    override fun requestForecastByZipCode(zipCode: Long, date: Long) =
            forecastDbHelper.use {
            ...
	}
	...
}
```

`ForecastServer` has not yet been implemented, but this is very simple. It will receive data from the server after the use of `ForecastDb` to save to the database. In this way, we can cache the data to the database, provided to the future of the query.

```kotlin
class ForecastServer(val dataMapper: ServerDataMapper = ServerDataMapper(),
        val forecastDb: ForecastDb = ForecastDb()) : ForecastDataSource {
        
    override fun requestForecastByZipCode(zipCode: Long, date: Long):
            ForecastList? {
        val result = ForecastByZipCodeRequest(zipCode).execute()
        val converted = dataMapper.convertToDomain(zipCode, result)
        forecastDb.saveForecast(converted)
        return forecastDb.requestForecastByZipCode(zipCode, date)
	}
}
```

It also uses the `data mapper` we created before. Finally, we changed the names of some of the functions to make it more similar to the mapper we used before in `database model`. You can view the `provider` to see the details.

The rewritten method is used to request the server to convert the results to `domain objects` and save them to the database. It finally queries the database to return the data, because we need to use the word id to insert into the database.

This is the last step in the implementation of the `provider`. Now we need to start using it. `ForecastCommand` will no longer interact directly with the server, nor will it convert the data to `domain model`.

```kotlin
RequestForecastCommand(val zipCode: Long,
        val forecastProvider: ForecastProvider = ForecastProvider()) :
        Command<ForecastList> {
        
	companion object {
		val DAYS = 7
	}
	
	override fun execute(): ForecastList {
		return forecastProvider.requestByZipCode(zipCode, DAYS)
	}
}
```

Other modifications include the renaming and the structural adjustment of the package. View the corresponding submission in [Kotlin for Android Developers repository].

[Kotlin for Android Developers repository]: https://github.com/antoniolg/Kotlin-for-Android-Developers
