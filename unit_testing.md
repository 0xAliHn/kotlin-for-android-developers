# Unit testing

I will not discuss the topic of `unit testing` (unit test). There are many definitions, but there are some subtle differences. A common view may be `unit testing` to verify the test of a unit (` unit`) source code. A unit (`unit`) contains nothing left to the reader. In our example, I just defined a `unit test` as a test that does not require a device to run. The IDE will run these tests and then show the final result to resolve which tests were successful and which tests failed.

`Unit testing` usually uses the` JUnit` library. So let's add this dependency to `build.gradle`. Because this dependency will only be used when running the test, so we can use `testCompile` instead of` compile`. In this way, the library will be ignored in the official compiler, you can reduce the size of APK:

```groovy
dependencies {
	...
	testCompile 'junit:junit:4.12'
}
```

Now synchronize gradle to get the library and add it to your project. To open `unit testing`, open the` Build Variants`tab (you may find it on the left side of the IDE) and click `Test Artifact`. You should select` Unit Tests`.

Another thing you need to do is create a new folder. Under src, you probably already have `androidTest` and` main` 's. Create another folder called `test`, and then create a` java` folder below it. So now you should have a folder named `src / test / java` green. This is the IDE that we are using the `Unit Test` model good signs that this folder will include some test files.

Let's write a very simple test to see if everything is running properly. Use the appropriate package name (my `com.antonioleiva.weatherapp`, but you need to use the main package name in your app) to create a new Kotlin class called` SimpleTest`. When you create it, write the following simple tests:

```kotlin
import org.junit.Test
import kotlin.test.assertTrue
class SimpleTest {
	@Test fun unitTestingWorks() {
	    assertTrue(true)
	}
}
```

Use the `@ Test` annotation to identify the function as a test. Confirm that it is `org.unit.Test`. Then add a simple assertion. It just judges whether true is true, it will obviously succeed. This test is only used to confirm that all configurations are correct.

Perform a test, just right-click on a folder in the new `java` file you created in the` test`, then select `Run All Tests`. When the compilation is complete, it runs the test and sees the display of the result profile. You should be able to see our test passed.

Now it's time to create a real test. All tests that are handled using the Android framework may require a `instrumentation test` or a more complex library like [Robolectric]. So in these examples I will not use the framework of anything. For example, I will test the extension function from `Long` to` String`.

Create a new file named `ExtensionTests` and add the following tests:

```kotlin
class ExtensionsTest {
	
	@Test fun testLongToDateString() {
        assertEquals("Oct 19, 2015", 1445275635000L.toDateString())
    }

	@Test fun testDateStringFullFormat() {
        assertEquals("Monday, October 19, 2015",
            1445275635000L.toDateString(DateFormat.FULL))
    }
}
```

These tests detect whether the `Long` instance can be converted to a` String`. The first test default behavior (using DateFormat.MEDIUM)), and the second one specifies a different format. Run these tests and then you will see them all through it. I suggest you modify them and see how they fail.

If you have used a test in Java, you will find that this is not much different. I will demonstrate a simple example where we can make some tests on `ForecastProvider`. We can use the `Mockito` library to simulate other classes and then test the provider independently:

```groovy
dependencies {
    ...
    testCompile "junit:junit:4.12"
    testCompile "org.mockito:mockito-core:1.10.19"
}
```

Now create a `ForecastProviderTest`. We're going to test `ForecastProvider` and use` DataSource` to return the result to see if it is null. So first we need to simulate a `ForecastDataSource`:

```kotlin
val ds = mock(ForecastDataSource::class.java)
`when`(ds.requestDayForecast(0)).then {
    Forecast(0, 0, "desc", 20, 0, "url")
}
```

As you can see, we need to put a counter-quotation on `when`. Because `when` is a reserved keyword in Kotlin, so if we use it in some Java code we need to avoid it. Now we use this data source to create a provider, and then detect whether the result of calling the method is null:

```kotlin
val provider = ForecastProvider(listOf(ds))
assertNotNull(provider.requestForecast(0))
```

This is the complete test function:

```kotlin
@Test fun testDataSourceReturnsValue() {
    val ds = mock(ForecastDataSource::class.java)
    `when`(ds.requestDayForecast(0)).then {
           Forecast(0, 0, "desc", 20, 0, "url")
    }
    
	val provider = ForecastProvider(listOf(ds))
	assertNotNull(provider.requestForecast(0))
}
```

If you run it, you will see that it will go wrong. Thanks to this test, we found some errors in our code. The test failed because the `ForecastProvider` was initialized in its` companion object` before it was used. We can add some data source to `ForecastProvider` by constructor, and this static List will never be used, so it should be loaded with` lazy`:

```kotlin
companion object {
	val DAY_IN_MILLIS = 1000 * 60 * 60 * 24
    val SOURCES by lazy { listOf(ForecastDb(), ForecastServer()) }
}
```

If you are going to run again now, you will find that all tests will now pass.
We can also test some, for example, when the data source returns null, it will facilitate the next data source to get the results:

```kotlin
@Test fun emptyDatabaseReturnsServerValue() {
    val db = mock(ForecastDataSource::class.java)
    val server = mock(ForecastDataSource::class.java)
    `when`(server.requestForecastByZipCode(
            any(Long::class.java), any(Long::class.java)))
            .then {
                ForecastList(0, "city", "country", listOf())
    val provider = ForecastProvider(listOf(db, server))
    assertNotNull(provider.requestByZipCode(0, 0))
}
```

As you can see, by using the default value of the parameter, this simple dependency is enough to allow us to implement some simple `unit tests`. There are a lot of things we can test for this provider, but this example is enough for us to learn to use the `unit testing` tool.

[Robolectric]: http://robolectric.org/
