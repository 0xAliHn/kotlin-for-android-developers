# The click listener for the ForecastListAdapter

In the previous chapter, I was so hard to write the click listener's purpose is to better develop in this chapter. But it is time to use what you have learned to go to practice. We removed the listener interface from the ForecastListAdapter and replaced it with lambda:

```kotlin
public class ForecastListAdapter(val weekForecast: ForecastList,
								 val itemClick: (Forecast) -> Unit)
```

The `itemClick` function takes a `forecast` parameter and does not return anything. `ViewHolder` can also be modified as follows:

```kotlin
class ViewHolder(view: View, val itemClick: (Forecast) -> Unit)
```

The other code remains the same. Just change `MainActivity`:

```kotlin
val adapter = ForecastListAdapter(result) { forecast -> toast(forecast.date) }
```

We can simplify the last sentence. If the function only receives a parameter, then we can use `it` reference, rather than to specify the parameters on the left. So we can do that:

```kotlin
val adapter = ForecastListAdapter(result) { toast(it.date) }
```
