# Start an activity: reified function

The last step is to start a `detail activity` from` main activity`. We can rewrite the adapter instance as follows:

```kotlin
val adapter = ForecastListAdapter(result) {
	val intent = Intent(MainActivity@this, javaClass<DetailActivity>())
	intent.putExtra(DetailActivity.ID, it.id)
	intent.putExtra(DetailActivity.CITY_NAME, result.city)
	startActivity(intent)
}
```

But it is very lengthy. As always, Anko provided a much simpler way to start an activity with 'reified function`'

```kotlin
val adapter = ForecastListAdapter(result) {
    startActivity<DetailActivity>(DetailActivity.ID to it.id,
            DetailActivity.CITY_NAME to result.city)
}
```

`Reified function` behind what magic in the end it? As you might know, when we create a paradigm in Java, we have no way to get the type of class. A popular workaround is passed as a parameter to a Class. In Kotlin, an inline (`inline`) function can be embodied (` reified`), which means that we can get and use the type of class in the function. Anko really uses a simple example of it to speak next (in this case I only used `String`):

```kotlin
public inline fun <reified T: Activity> Context.startActivity(
        vararg params: Pair<String, String>) {
    val intent = Intent(this, T::class.javaClass)
    params forEach { intent.putExtra(it.first, it.second) }
    startActivity(intent)
}
```

The real realization is more complicated because it uses a very long annoying `when` expression to increase the extra information determined by the type, but conceptually it does not add other more useful knowledge.

`Reified` function is a grammar that can simplify code and improve comprehension. In this example, it creates an intent, iterates all the parameters and adds to the intent, and then uses Intent to start the activity by getting the `javaClass` of the schema type. `Reified` is restricted to the subclass of activity.

The remaining details are already described in the code base. We now have a very simple (but complete) master-view (`master-detail`) App, which uses Kotlin implementation, does not use a line of Java code.
