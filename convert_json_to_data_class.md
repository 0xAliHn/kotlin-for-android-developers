#Convert json to data class

We now know how to create a data class, then we began to prepare to resolve the data. In the `date` package, create a new file named `ResponseClasses.kt`. If you open the url in Chapter 8, you can see the entire structure of the json file. Its basic composition includes a city, a series of weather forecasts, the city has id, name, where the coordinates. Every weather forecast has a lot of information, such as date, different temperatures, and an id by description and icon.

In our current UI, we will not use all of these data. We will parse all inside the class, because it may be used in some cases later. The following is the class we need to use:
```kotlin
data class ForecastResult(val city: City, val list: List<Forecast>)
data class City(val id: Long, val name: String, val coord: Coordinates,
                val country: String, val population: Int)
data class Coordinates(val lon: Float, val lat: Float)
data class Forecast(val dt: Long, val temp: Temperature, val pressure: Float,
                    val humidity: Int, val weather: List<Weather>,
                    val speed: Float, val deg: Int, val clouds: Int,
                    val rain: Float)
data class Temperature(val day: Float, val min: Float, val max: Float,
                       val night: Float, val eve: Float, val morn: Float)
data class Weather(val id: Long, val main: String, val description: String,
                   val icon: String)
```

When we use Gson to parse json into our class, the names of these attributes must be the same as those in json, or you can specify a `serialized name` (serialized name). A good practice is that most of the software structure will be based on our app layout to decouple into different models. So I like to use declarations to simplify these classes because I will parse these classes before using it in other parts of app. The name of the attribute is exactly the same as the name in the json result.

Now, in order to return the parsed results, our `Request` class needs to be modified. It will still only receive a city's `zipcode` as a parameter instead of a full url, so it becomes more readable. Now, I will put this static url in a `companion object` (with the object). If we have to add more requests to the API, we need to extract it.

> __Companion objects__
>> Kotlin allows us to define something that behaves like a static object. Although these objects can be implemented in well-known patterns, such as easy-to-implement singleton patterns.

>> We need a class which has some static properties, constants or functions, we can use the `companion object`. This object is shared by all the objects of this class, like static properties or methods in Java.

The following is the last code:

```kotlin
public class ForecastRequest(val zipCode: String) {
    companion object {
        private val APP_ID = "15646a06818f61f7b8d7823ca833e1ce"
        private val URL = "http://api.openweathermap.org/data/2.5/" +
                "forecast/daily?mode=json&units=metric&cnt=7"
        private val COMPLETE_URL = "$URL&APPID=$APP_ID&q="
	}
	
    fun execute(): ForecastResult {
        val forecastJsonStr = URL(COMPLETE_URL + zipCode).readText()
        return Gson().fromJson(forecastJsonStr, ForecastResult::class.java)
	}
}

```

Remember to add in the `build.gradle` you need Gson dependencies:

```groovy
compile "com.google.code.gson:gson:2.4"
```
