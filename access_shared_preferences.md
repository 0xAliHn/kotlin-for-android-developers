# Access Shared Preferences

You may know what is Android [Shared Preferences]. A simple set of key and value pairs that can be easily stored through the Android framework. These `preferences` are part of the SDK and make the task easier. And from Android 6.0 (Marshmallow), `shared preferences` can be automatically stored by the cloud, so when a user in a new device to restore the App, their` preferences` will be restored.

Thanks to the use of attribute delegation, we can use very simple way to deal with `preferences`. We can create a delegate, when `get` is called to query, when` set` is called to perform the save operation.

Because we want to save `zip code`, it is a long type, so let's create a Long attribute of the commission it. In `DelegatesExtensions.kt`, implement a new` LongPreference` class:

```kotlin
class LongPreference(val context: Context, val name: String, val default: Long)
    :  ReadWriteProperty<Any?, Long> {

	val prefs by lazy {
        context.getSharedPreferences("default", Context.MODE_PRIVATE)
    }

	override fun getValue(thisRef: Any?, property: KProperty<*>): Long {
        return prefs.getLong(name, default)
	}
	
	override fun setValue(thisRef: Any?, property: KProperty<*>, value: Long) {
        prefs.edit().putLong(name, value).apply()
    }
}
```

First, we use the `lazy` delegate to create a preferences. In this case, if we do not use this attribute, the delegate will not request the `SharedPreferences` object.

When `get` is called, its implementation is to use the preferences instance to get a long attribute for the specified name in the delegate declaration. If this attribute is not found, default is used. When a value is `set`, get the` preferences editor` and save it with the attribute name.

We can define a new delegate in `DelegatesExt`, so that when we visit a lot easier:

```kotlin
object DelegatesExt {
    ....
    fun longPreference(context: Context, name: String, default: Long) =
        LongPreference(context, name, default)
}
```

In `SettingActivity`, you can now define a property to handle `zip code` preferences. I created two constants for default values for names and attributes. This way can be used elsewhere in App:

```kotlin
companion object {
    val ZIP_CODE = "zipCode"
    val DEFAULT_ZIP = 94043L
}

var zipCode: Long by DelegatesExt.longPreference(this, ZIP_CODE, DEFAULT_ZIP)
```

It is very simple to work now, we can get from the attribute and assigned to the `EditText`:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    ...
    cityCode.setText(zipCode.toString())
}
```

We can not use the auto-generated property `text` because` EditText` returns `Editable` in `getText`, so the attribute defaults to that value. If I try to allocate a `String`, the compiler will error, using `setText()`is enough.

Now have all the things to achieve `onBackPressed`. Here, the new value of a property is stored:

```kotlin
override fun onBackPressed() {
    super.onBackPressed()
    zipCode = cityCode.text.toString().toLong()
}
```

`MainActivity` needs some minor changes. First, it also requires a `zip code` attribute.

```kotlin
val zipCode: Long by DelegatesExt.longPreference(this, SettingsActivity.ZIP_CODE,
            SettingsActivity.DEFAULT_ZIP)
```

Then I moved the `forecast load` code to` onResume`, so that every time the activities resumed, it refreshes the data to prevent `code zip` from being modified. Of course, there are more complex ways to do this, such as by asking the forecast before the test whether the `zip code` really changed. But I like to keep the simplicity of this example, and because the requested data has been stored in the local database, so this solution is not too bad:

```kotlin
override fun onResume() {
    super.onResume()
    loadForecast()
}

private fun loadForecast() = async {
    val result = RequestForecastCommand(zipCode).execute()
    uiThread {
	    val adapter = ForecastListAdapter(result) {
		    startActivity<DetailActivity>(DetailActivity.ID to it.id,
		            DetailActivity.CITY_NAME to result.city)
		}
		forecastList.adapter = adapter
		toolbarTitle = "${result.city} (${result.country})"
	} 
}
```

`RequestForecastCommand` now uses` zipCode` instead of the previous one is a fixed value.

There is also the opinion that we have to do things: when the overflow menu of the `settings` click to start the` setting activity`. The `initToolbar` function in` ToolbarManager` needs to have some minor changes:

```kotlin
when (it.itemId) {
    R.id.action_settings -> toolbar.ctx.startActivity<SettingsActivity>()
    else -> App.instance.toast("Unknown option")
}
```

[Shared Preferences]: http://developer.android.com/training/basics/data-storage/shared-preferences.html
