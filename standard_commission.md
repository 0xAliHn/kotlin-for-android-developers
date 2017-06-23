# Standard commission

There is a series of standard delegates in Kotlin's standard library. They include most useful delegates, but we can also create our own delegates.

#### Lazy

It contains a lambda, when the first implementation of `getValue` when the lambda will be called, so this property can be delayed initialization. Subsequent calls will only return the same value. This is a very interesting feature when we do not have to need them before they are actually called for the first time. We can save memory and do not initialize before these properties really need it.

```kotlin
class App : Application() {
    val database: SQLiteOpenHelper by lazy {
		MyDatabaseHelper(applicationContext)
	}

	override fun onCreate() {
	    super.onCreate()
	    val db = database.writableDatabase
    }
}
```

In this example, the database is not actually initialized until the first call to the `onCreate`. After that, we only ensure that the applicationContext exists and is ready to be used. The `lazy` operator is thread safe.

If you are not worried about multithreading or want to improve more performance, you can also use `lazy(LazyThreadSafeMode.NONE){ ... }`


#### Observable

This commission will help us monitor the changes we want to observe. When the `set` method of the observed property is called, it automatically executes the lambda expression we specified. So once the attribute is assigned a new value, we will receive the delegate attribute, the old value and the new value.

```kotlin
class ViewModel(val db: MyDatabase) {
	var myProperty by Delegates.observable("") {
	    d, old, new ->
	    db.saveChanges(this, new)
	}
}
```

This example shows that some of the ViewMode we need to care about, each time the value is modified, they will save them to the database.


#### Vetoable

This is a special `observable`, which lets you decide whether this value needs to be saved. It can be used to make some conditional judgments before it is actually saved.

```kotlin
var positiveNumber = Delegates.vetoable(0) {
    d, old, new ->
	new >= 0
}
```

The above delegate is only allowed to be saved when the new value is positive. In lambda, the last line represents the return value. You do not need to use the return keyword (essentially can not be compiled).

#### Not Null

Sometimes we need to initialize this property somewhere, but we can not be determined in the constructor, or we can not do anything in the constructor. The second case is common in Android: in the Activity, fragment, service, receivers ... ... In any case, a non-abstract attribute in the constructor before the implementation of the need to be assigned. In order to assign these attributes, we can not let it wait until we want to assign it to the time. We have at least two options.

The first is to use a nullable type and assign it to null until we have a value that really wants to be assigned. But we need to check in every place whether or not it is null. If we are sure that this property will not be null when we use it anymore, this may cause us to write some of the necessary code.

The second option is to use the `notNull` delegate. It will contain a nullable variable and will allocate a true value when we set this property. If the value is not allocated before it is fetched, it throws an exception.

This is useful in this example of a single case App:

```kotlin
class App : Application() {
    companion object {
        var instance: App by Delegates.notNull()
	}
	
	override fun onCreate() {
        super.onCreate()
        instance = this
	}
}
```

#### Map values from the Map

Another way to attribute the delegate is that the value of the attribute gets the value from a map, and the name of the attribute corresponds to the key in the map. This delegate allows us to do something very powerful because we can easily create an instance of an object from a dynamic map. If we import `kotlin.properties.getValue`, we can map from the constructor to the `val` property to get an unmodifiable map. If we want to modify the map and properties, we can also import `kotlin.properties.setValue`. The class requires a `MutableMap` as a constructor argument.

Imagine that we loaded a configuration class from a Json and then assigned their keys and values to a map. We can just by passing in a map constructor to create an instance:

```kotlin
import kotlin.properties.getValue

class Configuration(map: Map<String, Any?>) {
    val width: Int by map
    val height: Int by map
    val dp: Int by map
    val deviceName: String by map
}
```

As a reference, here I show under this class how to create a necessary map:

```kotlin
conf = Configuration(mapOf(
    "width" to 1080,
    "height" to 720,
    "dp" to 240,
    "deviceName" to "mydevice"
))
```
