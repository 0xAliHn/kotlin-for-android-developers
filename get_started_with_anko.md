# Get started with Anko

Before, let's use Anko to simplify some code. As you will see, any time you use some of the things in the Anko library, they will be imported in the form of attributes, methods, etc. This is because Anko uses the __extension function__ to add some new functionality to the Android framework. We will see in the future what is the extension function, how to write it.

In `MainActivity:onCreate`, an Anko extension function can be used to simplify the acquisition of a RecyclerView:

```kotlin
val forecastList: RecyclerView = find(R.id.forecast_list)
```

We can not use more of the library now, but Anko can help us to simplify the code, for example, instantiate Intent, Activity jump between, Fragment creation, database access, Alert creation ... we will To achieve this App process to learn a lot of interesting examples.
