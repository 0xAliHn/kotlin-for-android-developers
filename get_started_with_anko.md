# Start using Anko

Before going any further, let’s use Anko to simplify some code. As you will see, anytime you use
something from Anko, it will include an import with the name of the property or function to the file.
This is because Anko uses __extension function__ to add new features to Android framework. We’ll
see right below what an extension function is and how to write it.

In `MainActivity:onCreate`, an Anko extension function can be used to simplify how to find the `RecyclerView`:

```kotlin
val forecastList: RecyclerView = find(R.id.forecast_list)
```

We can’t use more from the library yet, but Anko can help us to simplify, among others, the
instantiation of intents, the navigation between activities, creation of fragments, database access,
alerts creation… We’ll find lots of interesting examples while we implement the App.
