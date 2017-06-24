# Instrumentation tests

`Instrumentation tests` is a little different. They are usually used in UI interaction, and we need an application instance to run while performing the tests. To achieve this, we need to deploy and run on the device.

This type of test must be placed in the `androidTest` folder, and we have to modify the` Test Variants`s ` Test Artifact` for `Android Instrumentation Tests`. The official realized instrumentation library is [Espresso], which by `Actions`,` filter` and `ViewMatchers` and` Matchers` test results can help us more easily use.

Configuration is more difficult than before. We need to download additional libraries and `Gradle` 's configuration. The good thing is that Kotlin's test does not need to add extra stuff, so if you already know how to configure `Espresso`, it will be very simple for you.

First, specify `test runner` in` defaultConfig` '

```groovy
defaultConfig {
    ...
    testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
}
```

When you have finished dealing with the runner, then add the other dependencies, this time with `androidTestCompile`. In this way, these libraries will only be compiled and run when the `instrumentation tests` is added:

```groovy
dependencies {
    ...
    androidTestCompile "com.android.support:support-annotations:$support_version"
    androidTestCompile "com.android.support.test:runner:0.4.1"
    androidTestCompile "com.android.support.test:rules:0.4.1"
    androidTestCompile "com.android.support.test.espresso:espresso-core:2.2.1"
    androidTestCompile ("com.android.support.test.espresso:espresso-contrib:2.2.1"){
	    exclude group: 'com.android.support', module: 'appcompat'
		exclude group: 'com.android.support', module: 'support-v4'
		exclude module: 'recyclerview-v7'
}
```

I do not want to spend a lot of time to talk about these, but here is why the need for a brief reason for these libraries:

- `support-annotations`: need to be used in other libraries.
- `runner`: This is` test runner`, that is what we specified in `defaultConfig` 's.
- `rules`: Include some rules that test inflate to start activity. We will use these rules in our example.
- `espresso-core`:` Espresso` 's basic implementation, it makes `instrument tests` easier.
- `espresso-contrib`: it adds other extra features, such as support for the` RecyclerView` test. We have to exclude some of its dependence, because we have been used in this project, otherwise the test will go wrong.

Let's now create a simple example. The test will click on the first row of the forecast list and it will determine if a view with the id is named `R.id.weatherDescription`. This view is in `DetailActivity`, which means that we can successfully navigate to the details page after testing in the RecyclerView.

```kotlin
class SimpleInstrumentationTest {

	@get:Rule
    val activityRule = ActivityTestRule(MainActivity::class.java)
    
    ...
}
```

First we need to specify it to run using `AndroidJUnit4`. Then, create an activity rule that instantiates the activity that a test requires. In Java, you can use `@ Rule`. But as you know, the fields and attributes are not the same, so if you use it like that, the execution will fail because the fields in the access property are not public. You need to add annotations to getter above. Kotlin allows you to specify `get` or` set` in front of the name of the rule. In this example, just `@get: Rule`.

After that we are ready to create the first test:

```kotlin
@Test fun itemClick_navigatesToDetail() {
    onView(withId(R.id.forecastList)).perform(
            RecyclerViewActions
                .actionOnItemAtPosition<RecyclerView.ViewHolder>(0, click()))
    onView(withId(R.id.weatherDescription))
           .check(matches(isAssignableFrom(TextView::class.java)))
}
```

Function with the `@ Test` annotation, this root we use the` unit test` the same way. We can start using `Espresso` in the test body. It first executed a click in the first position of `RecyclerView`. Then it detects whether you can find a view of the specified id and the view is a `TextView`.

To run this test, click on the top of the `Runencies drop-down select` Edit Configurations ... press the `+` icon, select `Android Tests`, then select the` app` module. Now, in the `target device` choose your favorite` target`. Click `OK` and run. You should be able to see how App is starting in your device, it will test the first position, open the details page and then close the app again.

Now we have to do something more complicated. Test will open from the banner of a overflow menu, click the `settings` column, change the city's` code`, and then check the toolbar of the title is changed to the corresponding title.

```kotlin
@Test fun modifyZipCode_changesToolbarTitle() {
	openActionBarOverflowOrOptionsMenu(activityRule.activity)
	onView(withText(R.string.settings)).perform(click())
	onView(withId(R.id.cityCode)).perform(replaceText("28830"))
	pressBack()
	onView(isAssignableFrom(Toolbar::class.java))
	        .check(matches(
	            withToolbarTitle(`is`("San Fernando de Henares (ES)"))))
}
```

This test actually does things:

- it first uses `openActionBarOverflowOrOptionsMenu` to open the overflow menu.
- then it looks for a view based on the `Settings` text and then clicks on it.
- After that, the setup interface will be opened, so it will look for a `EditText` and replace it with a new` code`.
- it will click the Back button. It will save the new value in the preferences, and then close the Activity.
- because the `mainActivity`, `onResume` will be called, the request will be called again. At this point it will get the forecast for the new city.
- the last line will detect the Toolbar we see whether the title is the title of the new city.

This is not the default matcher for a toolbar title, but `Espresso` is very easy to expand, so we can create a new matcher to implement the detection:

```kotlin
private fun withToolbarTitle(textMatcher: Matcher<CharSequence>): Matcher<Any> =
        object : BoundedMatcher<Any, Toolbar>(Toolbar::class.java) {

	override fun matchesSafely(toolbar: Toolbar): Boolean {
		return textMatcher.matches(toolbar.title)
	}

	override fun describeTo(description: Description) {
		description.appendText("with toolbar title: ")
		textMatcher.describeTo(description)
	}                
}
```

`MatchSafely` function is where we detect, and` describeTo` function adds some new information to matcher.

This chapter is particularly interesting because we see how to integrate tests in Kotlin perfectly, and they can integrate the test without any problems. Look at the code and run your own.


[Espresso]: https://google.github.io/android-testing-support-library/
