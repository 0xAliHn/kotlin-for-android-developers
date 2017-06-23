# Rebuild our code

Now it's time to use `Kotlin Android Extensions` to modify our code. The modification is fairly simple.

We start with `MainActivity`. We are currently using only the `forecastList` of RecyclerView. But we can simplify the code. First, add manual for `activity_main`XML import:

```kotlin
import kotlinx.android.synthetic.activity_main.*
```

As mentioned before, we use id to access views. So I want to modify the `RecyclerView` id, do not use the underscore, making it more suitable for the Kotlin variable name. XML last as follows:

```xml
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/forecastList"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</FrameLayout>
```

And now, we can not need `find` this line:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    forecastList.layoutManager = LinearLayoutManager(this)
    ...
}
```

This is already the smallest simplification, because this layout is very simple. But `ForecastListAdapter` can also benefit from this plugin. Here you can use a device to bind these properties to the view, it can help us remove all `ViewHolder` of`find` code.

First, add a manual import for `item_forecast`:

```kotlin
import kotlinx.android.synthetic.item_forecast.view.*
```

Then we can now use the properties contained in `itemView` in `ViewHolder` . In fact, you can use these properties in any view, but it is clear that if the view does not contain the sub view to get will crash.

Now we can directly access the view of the property:

```kotlin
class ViewHolder(view: View, val itemClick: (Forecast) -> Unit) :
        RecyclerView.ViewHolder(view) {
    fun bindForecast(forecast: Forecast) {
        with(forecast){
	        Picasso.with(itemView.ctx).load(iconUrl).into(itemView.icon)
			itemView.date.text = date
			itemView.description.text = description
			itemView.maxTemperature.text = "${high.toString()}"
			itemView.minTemperature.text = "${low.toString()}"
			itemView.onClick { itemClick(forecast) }
		} 
	}
}
```

The Kotlin Android Extensions plugin helps us to reduce a lot of template code and simplify the way we visit view. Check out the latest code from the library.
