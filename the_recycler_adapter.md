# The Recycler Adapter

We need an adapter for the recycler too. I [talked about `RecyclerView` before my blog], some time ago,
so it may help you if your are not used to it.

The views used for `RecyclerView` adapter will be just  `TextView`, for now, and a simple list of texts
that we’ll create manually. Add a new Kotlin file called `ForecastListAdapter.kt`, and include this
code:

```kotlin
class ForecastListAdapter(val items: List<String>) :
        RecyclerView.Adapter<ForecastListAdapter.ViewHolder>() {
        
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        return ViewHolder(TextView(parent.context))
    }
    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        holder.textView.text = items.[position]
    }
    
    override fun getItemCount(): Int = items.size
    
    class ViewHolder(val textView: TextView) : RecyclerView.ViewHolder(textView)
}
```

Again, we can access to the context and the text as properties. You can keep doing it as usual (using
getters and setter), but you’ll get a warning from the compiler. This check can be disabled if you
prefer to keep using the Java way. Once you get used to properties you will love them anyway.

Back to `MainActivity`, now just create the list of strings and then assign the adapter:

```kotlin
private val items = listOf(
    "Mon 6/23 - Sunny - 31/17",
    "Tue 6/24 - Foggy - 21/8",
    "Wed 6/25 - Cloudy - 22/17",
    "Thurs 6/26 - Rainy - 18/11",
    "Fri 6/27 - Foggy - 21/10",
    "Sat 6/28 - TRAPPED IN WEATHERSTATION - 23/18",
    "Sun 6/29 - Sunny - 20/7"
    )
    
override fun onCreate(savedInstanceState: Bundle?) {
    ...
    val forecastList = findViewById(R.id.forecast_list) as RecyclerView
    forecastList.layoutManager = LinearLayoutManager(this) 
    forecastList.adapter = ForecastListAdapter(items)
}
    
```

>List created
>>Though I’ll talk about collections later on this book, I just want to explain for now that you
can create constant lists (what we will see as immutable soon) by using the helper function `listOf` It receives a `vararg` of items of any type and infers the type of the result.

>> There are many other alternative functions, such as `setOf`, `arrayListOf` or `hashSetOf` among others.

I also moved some classes to new packages, in order to improve organisation.

We reviewed many new ideas in such a small amount of code, so I’ll be covering them in the next
chapter. We can’t continue until we learn some important concepts regarding basic types, variables
and properties.



[I talked about `RecyclerView` before my blog]: http://antonioleiva.com/recyclerview/
