# Draw the data in the UI

`MainActivity` in the code some minor changes, because now there is real data needs to be filled into the adapter. Asynchronous calls need to be rewritten as:

```kotlin
async() {
	val result = RequestForecastCommand("94043").execute()
	uiThread{
		forecastList.adapter = ForecastListAdapter(result)
	}
}
```

Adapter also needs to be modified:

```kotlin
class ForecastListAdapter(val weekForecast: ForecastList) :
        RecyclerView.Adapter<ForecastListAdapter.ViewHolder>() {
        
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int):
            ViewHolder? {
        return ViewHolder(TextView(parent.getContext()))
    }
        
    override fun onBindViewHolder(holder: ViewHolder,
            position: Int) {
        with(weekForecast.dailyForecast[position]) {
            holder.textView.text = "$date - $description - $high/$low"
	} 
}
    override fun getItemCount(): Int = weekForecast.dailyForecast.size
    
    class ViewHolder(val textView: TextView) : RecyclerView.ViewHolder(textView)
}
```

> __with function__
> > With is a very useful function that is included in Kotlin's standard library. It takes an object and an extension function as its argument, and then causes the object to extend the function. This means that all of the code we write in parentheses is an extension function of the object (the first argument), and we can use all of its public methods and properties just like this. This is very useful for simplifying the code when we do a lot of work on the same object.

In this chapter there are many new code to join, so check out the [library] code bar.

[library]: https://github.com/antoniolg/Kotlin-for-Android-Developers
