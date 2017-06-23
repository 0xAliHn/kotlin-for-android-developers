# Application

According to the simplest way we create a single case in Java like this:
```kotlin
class App : Application() {
    companion object {
        private var instance: Application? = null
	    fun instance() = instance!!
    }
    override fun onCreate() {
        super.onCreate()
        instance = this
	}
}
```

For this Application instance to be used, remember to add this `App` in` AndroidManifest.xml '

```xml
<application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:theme="@style/AppTheme"
    android:name=".ui.App">
    ...
</application>
```

Android has a problem, that is, we can not control many types of constructors. For example, we can not initialize a non-null attribute because its value needs to be defined in the constructor. So we need a nullable variable, and a function that returns a non-null value. We know that we always have an instance of `App`, but we can not do anything before it calls` onCreate`, so for security purposes, we assume that the `instance()` function will always return a non-null `App` instance.

But the program looks a little unnatural. We need to define a property (already getter and setter), and then through a function to return to that property. Do we have other ways to achieve similar effects? Yes, we can delegate the value of this attribute to another class. This is what we know the `delegate attribute'.
