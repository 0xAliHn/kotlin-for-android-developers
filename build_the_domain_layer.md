# Build the domain layer

We now create a new package as the `domain` layer. This layer will contain some `Commands` implementation to perform tasks for the app.

First, you must define a `Command`:

```kotlin
public interface Command<T> {
	fun execute(): T
}
```

The command will perform an operation and return a certain type of object, which can be specified by the pattern. You need to know an interesting concept, __everything kotlin function will return a value__. If it is not specified, it will return a `Unit` class by default. So if we want Command not to return the data, we can specify it for Unit.

Kotlin interfaces are much more powerful than Java (before Java 8) because they can contain code. But we do not need more code now, in the next chapter will be carefully talk about this topic.

The first command needs to request the weather forecast structure and then convert the result to the domain class. The following classes are defined in the domain class:

```kotlin
data class ForecastList(val city: String, val country: String,
                        val dailyForecast:List<Forecast>)

data class Forecast(val date: String, val description: String, val high: Int,
                    val low: Int)
```

When more features are added, these classes may need to be reviewed later. But now these classes are enough for us.

These classes must map from the data to our domain class, so I need to create a `DataMapper`:

```kotlin
public class ForecastDataMapper {
    fun convertFromDataModel(forecast: ForecastResult): ForecastList {
        return ForecastList(forecast.city.name, forecast.city.country,
                convertForecastListToDomain(forecast.list))
    private fun convertForecastListToDomain(list: List<Forecast>):
            List<ModelForecast> {
        return list.map { convertForecastItemToDomain(it) }
    }
    private fun convertForecastItemToDomain(forecast: Forecast): ModelForecast {
        return ModelForecast(convertDate(forecast.dt),
                forecast.weather[0].description, forecast.temp.max.toInt(),
                forecast.temp.min.toInt())
}
    private fun convertDate(date: Long): String {
        val df = DateFormat.getDateInstance(DateFormat.MEDIUM, Locale.getDefault())
		return df.format(date * 1000)
	}
}
```

When we use two classes with the same name, we can assign an alias to one of them, so we do not need to write the full package name:

```kotlin
import com.antonioleiva.weatherapp.domain.model.Forecast as ModelForecast
```

Another interesting thing about this code is how we convert from a forecast list to a domain model:

```kotlin
return list.map { convertForecastItemToDomain(it) }
```

With this statement, we can loop through the collection and return a new list of conversions. Kotlin provides a lot of nice function operators in the List that can be applied to each item in this List and convert them in any way. Compare Java 7, which is one of Kotlin's powerful features. We will soon see all the different operators. Knowing their presence is important because they are much more convenient and can save a lot of time and templates.

Now, ready before writing the command:

```kotlin
class RequestForecastCommand(val zipCode: String) :
				Command<ForecastList> {
	override fun execute(): ForecastList {
	    val forecastRequest = ForecastRequest(zipCode)
	    return ForecastDataMapper().convertFromDataModel(
	            forecastRequest.execute())
	}
}
```
