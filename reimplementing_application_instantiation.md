# Reimplementing Application instantiation

In this scenario, the commission can help us. We will not be null until our singleton is null, but we can not use the constructor to initialize the property. So we can use `notNull` delegate:

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

In this case there is a problem, we can anywhere in the app to modify this value, because if we use `Delegates.notNull()`, the property must be var. But we can use the delegate just created, so that you can protect a little more. We can only modify this value once:

```kotlin
companion object {
	var instance: App by DelegatesExt.notNullSingleValue()
}
```

Although, in this case, using a single case may be the easiest way, but I would like to show you how to create a custom delegate in the form of code.
