# Make Forecast list available

As a real app, the current list of each item layout should do some work. The first thing is to create a suitable XML, to meet our needs on the line. We want to display an icon, date, description and maximum and minimum temperatures. So let's create a layout called `item_forecast.xml`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
	xmlns:android="http://schemas.android.com/apk/res/android"
	xmlns:tools="http://schemas.android.com/tools"
	android:layout_width="match_parent"
	android:layout_height="match_parent"
	android:padding="@dimen/spacing_xlarge"
	android:background="?attr/selectableItemBackground"
	android:gravity="center_vertical"
	android:orientation="horizontal">
	
	<ImageView
		android:id="@+id/icon"
		android:layout_width="48dp"
		android:layout_height="48dp"
		tools:src="@mipmap/ic_launcher"/>
	
	<LinearLayout
		android:layout_width="0dp"
		android:layout_height="wrap_content"
		android:layout_weight="1"
		android:layout_marginLeft="@dimen/spacing_xlarge"
		android:layout_marginRight="@dimen/spacing_xlarge"
		android:orientation="vertical">
	
	<TextView
		android:id="@+id/date"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:textAppearance="@style/TextAppearance.AppCompat.Medium"
		tools:text="May 14, 2015"/>
	
	<TextView
		android:id="@+id/description"
		android:layout_width="match_parent"
		android:layout_height="wrap_content"
		android:textAppearance="@style/TextAppearance.AppCompat.Caption"
		tools:text="Light Rain"/>
	
	</LinearLayout>
	<LinearLayout
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:gravity="center_horizontal"
		android:orientation="vertical">
	
	<TextView
		android:id="@+id/maxTemperature"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:textAppearance="@style/TextAppearance.AppCompat.Medium"
		tools:text="30"/>
	
	<TextView
		android:id="@+id/minTemperature"
		android:layout_width="wrap_content"
		android:layout_height="wrap_content"
		android:textAppearance="@style/TextAppearance.AppCompat.Caption"
		tools:text="15"/>
	
	</LinearLayout>
</LinearLayout>
```

Domain model and data mapping must generate a complete icon uil, so we can do this to load it:

```kotlin
data class Forecast(val date: String, val description: String,
					val high: Int, val low: Int, val iconUrl: String)
```

In `ForecastDataMapper`:

```kotlin
private fun convertForecastItemToDomain(forecast: Forecast): ModelForecast {
    return ModelForecast(convertDate(forecast.dt),
            forecast.weather[0].description, forecast.temp.max.toInt(),
            forecast.temp.min.toInt(), generateIconUrl(forecast.weather[0].icon))
}

private fun generateIconUrl(iconCode: String): String
        = "http://openweathermap.org/img/w/$iconCode.png"
```

We get the icon from the first request code, used to form the completion of the icon url. The easiest way to load an image is to use the image load library. [`Picasso`] is a good choice. It needs to be added to the `build.gradle` dependency:

```groovy
compile "com.squareup.picasso:picasso:<version>"
```

So, Adapter also need a big change. Also need a click listener, we have to define it:

```kotlin
public interface OnItemClickListener {
     operator fun invoke(forecast: Forecast)
}
```

If you still remember the previous lesson, the `invoke` method can be omitted when called. So let's use it to simplify it. The listener can be called in two ways:

```kotlin
itemClick.invoke(forecast)
itemClick(forecast)
```

`ViewHolder` will be responsible for binding data to the new View:

```kotlin
class ViewHolder(view: View, val itemClick: OnItemClickListener) :
				RecyclerView.ViewHolder(view) {
    private val iconView: ImageView
    private val dateView: TextView
    private val descriptionView: TextView
    private val maxTemperatureView: TextView
    private val minTemperatureView: TextView

    init {
    	iconView = view.find(R.id.icon)
    	dateView = view.find(R.id.date)
    	descriptionView = view.find(R.id.description)
    	maxTemperatureView = view.find(R.id.maxTemperature)
    	minTemperatureView = view.find(R.id.minTemperature)
    }

    fun bindForecast(forecast: Forecast) {
    	with(forecast) {
    	    Picasso.with(itemView.ctx).load(iconUrl).into(iconView)
    	    dateView.text = date
    	    descriptionView.text = description
    	    maxTemperatureView.text = "${high.toString()}"
    	    minTemperatureView.text = "${low.toString()}"
    	    itemView.setOnClickListener { itemClick(forecast) }
	    }
    }
}
```

Now the constructor of the Adapter receives a `itemClick`. Creating and binding data is also simpler:

```kotlin
public class ForecastListAdapter(val weekForecast: ForecastList,
         val itemClick: ForecastListAdapter.OnItemClickListener) :
        RecyclerView.Adapter<ForecastListAdapter.ViewHolder>() {
        
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int):
            ViewHolder {
        val view = LayoutInflater.from(parent.ctx)
            .inflate(R.layout.item_forecast, parent, false)
        return ViewHolder(view, itemClick)
    }
    
    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.bindForecast(weekForecast[position])
	}
	...
}
```

If you use the above code, `parent.ctx` will not be compiled successfully. Anko offers a number of extended functions to make Android programming easier. For example, the activitys, fragments, and others contain the `ctx` attribute, which returns the context through the `ctx` attribute, but is missing this property in the View. So we want to create a new name called `ViewExtensions.kt` file instead of `ui.utils`, and then add this extended attribute:

```kotlin
val View.ctx: Context
    get() = context
```

From now on, any View can use this property. This is not necessary because you can use the extended context attribute, but I think that if we use `ctx`, it will be more coherent in other classes. And that's a good example of how to use extended attributes.

Finally, the MainActivity calls the setAdapter, and the result is this:
```kotlin
forecastList.adapter = ForecastListAdapter(result,
        object : ForecastListAdapter.OnItemClickListener{
			override fun invoke(forecast: Forecast) {
			    toast(forecast.date)
		    }
	    })
```

As you can see, to create an anonymous inner class, we went to create an implementation of the interface just created the object. Looks not very good, right? This is because we have not started experimenting with another powerful functional programming feature, but you will learn how to convert the code to the simpler in the next chapter.

Go to the codebase to update the new code. UI starts to look better.

[`Picasso`]: http://square.github.io/picasso/
