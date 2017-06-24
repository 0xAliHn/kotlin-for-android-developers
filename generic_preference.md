# Generic preference

Now that we are already a generic expert, why not extend `LongPreference` to support all` Shared Preferences` support types? We come to create a `Preference` delegate:

```kotlin
class Preference<T>(val context: Context, val name: String, val default: T)
    : ReadWriteProperty<Any?, T> {
	
	val prefs by lazy {
	    context.getSharedPreferences("default", Context.MODE_PRIVATE)
    }
	
	override fun getValue(thisRef: Any?, property: KProperty<*>): T {
		return findPreference(name, default)
	}

	override fun setValue(thisRef: Any?, property: KProperty<*>, value: T) {
        putPreference(name, value)
    }
	...
}
```

This preference is very similar to what we used before. We only replaced the `long` for the generic type` T`, and then called the two functions to do the specific important work. These functions are very simple, although some repetitions. They will check the type and then use the specified way to operate. For example, the `findPrefernce` function is as follows:

```kotlin
private fun <T> findPreference(name: String, default: T): T = with(prefs) {
        val res: Any = when (default) {
        is Long -> getLong(name, default)
        is String -> getString(name, default)
        is Int -> getInt(name, default)
        is Boolean -> getBoolean(name, default)
        is Float -> getFloat(name, default)
        else -> throw IllegalArgumentException(
            "This type can be saved into Preferences")
    }

	res as T
}
```

` PutPreference` function is the same, but in `when` Finally through` apply`, use the `preferences editor` to save the results:

```kotlin
private fun <U> putPreference(name: String, value: U) = with(prefs.edit()) {
    when (value) {
        is Long -> putLong(name, value)
        is String -> putString(name, value)
        is Int -> putInt(name, value)
        is Boolean -> putBoolean(name, value)
        is Float -> putFloat(name, value)
        else -> throw IllegalArgumentException("This type can be saved into Pref\
erences")
    }.apply()
}
```

Now modify the `DelegateExt`:

```kotlin
object DelegatesExt {
	...
	fun preference<T : Any>(context: Context, name: String, default: T)
	    = Preference(context, name, default)
}
```

After this chapter, the user can access the setup interface and modify the `zip code`. Then when we return to the main interface, the forecast will automatically re-refresh and display the new information. View the remaining xi wei in the code base
