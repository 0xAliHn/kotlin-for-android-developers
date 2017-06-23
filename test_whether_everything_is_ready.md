# Test whether everything is ready

We would like to write some code to test whether the Kotlin Android Extensions is working. I will not explain the code now, but I want to make sure they are working properly in your environment. This may be the hardest part of the configuration.

First, open `activity_main.xml` and set the id of the TextView:
```xml
<TextView
    android:id="@+id/message"
    android:text="@string/hello_world"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
```

Then, manually add an import statement to the Activity (do not worry about what you do not understand).

```kotlin
import kotlinx.android.synthetic.main.activity_main.*
```

In `onCreate`, you can now get and access this TextView directly.

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    message.text = "Hello Kotlin!"
}
```

Thanks to the interoperability between Kotlin and Java, we can manipulate the getter / setter methods in the Java library in Kotlin like operating attributes. We will go to explain the properties later, but I would like to remind that we can use `message.text` instead of` message.setText`. The compiler will convert it to the general Java code, so the use is not any performance overhead.

Now run the app, and it is running normally. Check whether the TextView is showing new content. If you have questions or want to see the code, please check out the [Kotlin for Android Developers repository]. Each section as long as the modified code, I will be submitted, so be sure to check all the code changes.

The next chapter will cover the new things you see in the MainActivity after the conversion. Once you understand the subtle changes between Java and Kotlin, you will be able to write new code more easily.


[Kotlin for Android Developers repository]: https://github.com/antoniolg/Kotlin-for-Android-Developers


