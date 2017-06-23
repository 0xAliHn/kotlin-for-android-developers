# example

You can imagine that the Kotlin List implements the array operator, so we can access each item of the List like an array in Java. In addition: In the modified List, each item can also be set directly in a simple way:

```kotlin
val x = myList[2]
myList[2] = 4
```

If you remember, we have a data class called ForecastList, which is made up of a lot of other extra information. Interestingly it is possible to access each item directly instead of asking the inside of the list to get an item. Do a completely irrelevant thing, I'm going to implement a `size()` method that can slightly simplify the current adapter:

```kotlin
data class ForecastList(val city: String, val country: String,
                        val dailyForecast: List<Forecast>) {
    operator fun get(position: Int): Forecast = dailyForecast[position]
    fun size(): Int = dailyForecast.size
}
```

It will make our `onBindViewHolder` much simpler:

```kotlin
override fun onBindViewHolder(holder: ViewHolder,
        position: Int) {
	with(weekForecast[position]) {
	    holder.textView.text = "$date - $description - $high/$low"
	}
}
```

Of course there are `getItemCount()` methods:

```kotlin
override fun getItemCount(): Int = weekForecast.size()
```
