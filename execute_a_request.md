# Execute a request

For the idea we are trying to achieve, our current text is a good start, but it is time to ask for some of the real data displayed on the RecyclerView. We will use the [OpenWeatherMap] API to get the data, there are some common class to the reality of this request. Thanks to Kotlin's very powerful interoperability, you can use any library you want to use, such as using [Retrofit] to execute a server request. When you just execute a simple API request, we can simply use any third party library to implement it simply.

And, as you can see, Kotlin provides some extension functions to make the request easier. First, we want to create a new Request class:

```kotlin
public class Request(val url: String) {
    public fun run() {
        val forecastJsonStr = URL(url).readText()
        Log.d(javaClass.simpleName, forecastJsonStr)
    }
}
```

Our request simply receives a url, then reads the result and prints json on logcat. The implementation is very simple, because we use `readText`, which is an extension function in the Kotlin standard library. This method does not recommend a great response to the results, but it is good enough in our example.

If you use this code to compare Java, you will find that we only use the standard library to save a lot of code. Such as `HttpURLConnection`, `BufferedReader` and the iteration results necessary to achieve the same effect, manage the connection status, reader and other parts of the code. Obviously, these are the things behind the scenes, but we do not care.

In order to be able to perform the request, App must have Internet permissions. So need to add in `AndroidManifest.xml` :

```xml
<uses-permission android:name="android.permission.INTERNET" />
```


[OpenWeatherMap]: http://openweathermap.org/
[Retrofit]: https://github.com/square/retrofit
