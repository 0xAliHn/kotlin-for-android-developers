# Performing A Request

Our current placeholder texts are good to start feeling the idea of what we want to achieve, but
now it’s time to request some real data, which will be used to populate the `RecyclerView`. We’ll be
using [OpenWeatherMap] API to retrieve data, and some regular classes for the request. As Kotlin
interoperability is extremely powerful, you could use any library you want, such as  [Retrofit], for
server requests. However, as we are just performing a simple API request, we can easily achieve our
goal much easier without adding another third party library.

Besides, as you will see, Kotlin provides some extension functions that will make requests much
easier. First, we’re going to create a new  Request class:

```kotlin
public class Request(val url: String) {
    public fun run() {
        val forecastJsonStr = URL(url).readText()
        Log.d(javaClass.simpleName, forecastJsonStr)
    }
}
```

Our request simply receives an url, reads the result and outputs the json in the Logcat. The
implementation is really easy when using `readText`, an extension function from the Kotlin standard
library. This method is not recommended for huge responses, but it will be good enough in our case.
                                                     
If you compare this code with the one you’d need in Java, you will see we’ve saved a huge amount of
overhead just using the standard library. An `HttpURLConnection`, `BufferedReader` and an iteration
over the result would have been necessary to get the same result, apart from having to manage the
status of the connection and the reader. Obviously, that’s what the function is doing behind the
scenes, but we have it for free.
                                                                                   
In order to be able to perform the request, the App must use the Internet permission. So it needs to
be added to the `AndroidManifest.xml` :

```xml
<uses-permission android:name="android.permission.INTERNET" />
```


[OpenWeatherMap]: http://openweathermap.org/
[Retrofit]: https://github.com/square/retrofit
