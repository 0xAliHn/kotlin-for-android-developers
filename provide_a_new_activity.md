# Provide a new activity

Now we are going to create a `DetailActivity`. Our details activity will receive a set of parameters passed from the main activity: `forecast id` and `city name`. The first argument will be used to request data from the database, and the city name is used to display on the toolbar. So we first need to define the name of a set of parameters:

```kotlin
public class DetailActivity : AppCompatActivity() {
	companion object {
	    val ID = "DetailActivity:id"
	    val CITY_NAME = "DetailActivity:cityName"
    }
    ...
}
```

In the `onCreate` function, the first step is to set the content view. UI is very simple, but for the app is enough:

```kotlin
<LinearLayout
	xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:tools="http://schemas.android.com/tools"
	android:layout_width="match_parent"
	android:layout_height="match_parent"
	android:orientation="vertical"
	android:paddingBottom="@dimen/activity_vertical_margin"
	android:paddingLeft="@dimen/activity_horizontal_margin"
	android:paddingRight="@dimen/activity_horizontal_margin"
	android:paddingTop="@dimen/activity_vertical_margin">
	
	<LinearLayout
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:orientation="horizontal"
		android:gravity="center_vertical"
		tools:ignore="UseCompoundDrawables">
	
		<ImageView
			android:id="@+id/icon"
			android:layout_width="64dp"
			android:layout_height="64dp"
			tools:src="@mipmap/ic_launcher"
			tools:ignore="ContentDescription"/>
		
		<TextView
			android:id="@+id/weatherDescription"
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:layout_margin="@dimen/spacing_xlarge"
			android:textAppearance="@style/TextAppearance.AppCompat.Display1"
		    tools:text="Few clouds"/>
    </LinearLayout>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <TextView
            android:id="@+id/maxTemperature"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:layout_margin="@dimen/spacing_xlarge"
            android:gravity="center_horizontal"
            android:textAppearance="@style/TextAppearance.AppCompat.Display3"
            tools:text="30"/>
        <TextView
            android:id="@+id/minTemperature"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:layout_margin="@dimen/spacing_xlarge"
            android:gravity="center_horizontal"
            android:textAppearance="@style/TextAppearance.AppCompat.Display3"
            tools:text="10"/>
    </LinearLayout>
</LinearLayout>
```

And then set it in the `onCreate` code. Use the name of the city to set the title of the toolbar. `Intent` and` title` are automatically mapped to attributes by the following method:

```kotlin
setContentView(R.layout.activity_detail)
title = intent.getStringExtra(CITY_NAME)
```

`Another part of the implementation of` onCreate` is to call command. This is very similar to what we did before:

```kotlin
async {
       val result = RequestDayForecastCommand(intent.getLongExtra(ID, -1)).execute()
       uiThread { bindForecast(result) }
}
```

When the result is fetched from the database, the `bindForecast` function is called in the UI thread. We have used the Kotlin Android Extensions plugin again in this activity to do not use `findViewById` to get the property from XML:

```kotlin
import kotlinx.android.synthetic.activity_detail.*
...

private fun bindForecast(forecast: Forecast) = with(forecast) {
       Picasso.with(ctx).load(iconUrl).into(icon)
       supportActionBar.subtitle = date.toDateString(DateFormat.FULL)
       weatherDescription.text = description
       bindWeather(high to maxTemperature, low to minTemperature)
}
```

Here are some interesting places. For example, I created another extension function to convert a `Long` object to a date string for display. Remember that we are also used in the adapter, so clearly defined it as a function is a good practice:

```kotlin
fun Long.toDateString(dateFormat: Int = DateFormat.MEDIUM): String {
    val df = DateFormat.getDateInstance(dateFormat, Locale.getDefault())
    return df.format(this)
}
```

I'll get a `date format` (or use the default DateFormat.MEDIUM) and converted into a` Long` user can understand `String`.

Another interesting place is the `bindWeather` function. It receives a `Int`` pairs` and `TextView`` vararg` a composition, and to set a different `TextView`` text` and `text color` accordance with the temperature.

```kotlin
private fun bindWeather(vararg views: Pair<Int, TextView>) = views.forEach {
    it.second.text = "${it.first.toString()}￿￿"
    it.second.textColor = color(when (it.first) {
        in -50..0 -> android.R.color.holo_red_dark
        in 0..15 -> android.R.color.holo_orange_dark
        else -> android.R.color.holo_green_dark
	})
}
```

Each pair, it will set a `text` to show the temperature and a different color according to the temperature match: low temperature with red, medium temperature with orange, the other with green. The temperature value is relatively random, but this is a good representation of using the `when` expression to make the code thinner.

`Color` is an extension function I miss Anko in, it can be very simple way to get a color from resources, similar to our use elsewhere` dimen`. When we write this line, the current `support library` relies on` ContextCompat` to get a color from a different Android version:

```kotlin
public fun Context.color(res: Int): Int = ContextCompat.getColor(this, res)
```

`AndroidManifest` also needs to know the existence of new activity:

```kotlin
<activity
    android:name=".ui.activities.DetailActivity"
    android:parentActivityName=".ui.activities.MainActivity" >
    <meta-data
        android:name="android.support.PARENT_ACTIVITY"
        android:value="com.antonioleiva.weatherapp.ui.activities.MainActivity" />
</activity>
```
