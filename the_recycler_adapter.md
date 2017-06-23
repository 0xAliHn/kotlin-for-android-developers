# The Recycler Adapter

We also need a `RecyclerView` Adapter. [I talked about `RecyclerView` before my blog], and it might be helpful if you have not used it before.

`RecyclerView` used in the layout now only need a `TextView`, I will manually create this simple text list. Add a Kotlin file named `ForecastListAdapter.kt`, including the following code:
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

Again, we can access context and text like access properties. You can keep the previous operations (using getters and setters), but you will get a compiler warning. If you still prefer to use it in Java, this check can be turned off. But once you use the property call on the way you will fall in love with it, and it also saves the total amount of extra characters.

Back to `MainActivity`, now simply create a series of String into the List, and then use the Create Assignment Adapter instance.

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
>>Although I will explain the Collection later in this book, I will simply explain that you can create a constant List by using a function `listOf` (soon we will speak of `immutable`). It receives a type of `vararg` (variable length parameter), it will automatically infer the type of result.
>> There are many other functions that can be selected, such as `setOf`, `arrayListOf` or `hashSetOf`.

In order to optimize the structure of the project, I also moved some classes into the new package inside.

We've seen a lot of new stuff in the very short code above, so I'll talk about them in the next chapter. We must now learn some basic concepts such as basic types, variables, attributes and so on to continue.



[I talked about `RecyclerView` before my blog]: http://antonioleiva.com/recyclerview/
