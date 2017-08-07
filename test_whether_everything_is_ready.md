# Test whether everything is ready

We’re going to add some code to test Kotlin Android Extensions are working. I’m not explaining
much about it yet, but I want to be sure this is working for you. It’s probably the trickiest part in
this configuration.

First, go to `activity_main.xml` and set an id for the TextView:
```xml
<TextView
    android:id="@+id/message"
    android:text="@string/hello_world"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
```

Now, add the synthetic import to the activity (don’t worry if you don’t understand much about it
yet):

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

Thanks to Kotlin interoperability with Java, we can use setters and getters methods from Java
libraries as a property in Kotlin. We’ll talk about properties later, but just notice that we can use `message.text` instead of `message.setText` for free. The compiler will use the real Java methods, so
there’s no performance overhead when using it.

Now run the app and see everything it’s working fine. Check that the message TextView is showing
the new content. If you have any doubts or want to review some code, take a look at
[Kotlin for Android Developers repository]. Each section as long as the modified code, I will be submitted, so be sure to check all the code changes.I’ll be adding a new commit for every chapter, when the chapter
implies changes in code, so be sure to review it to check all the changes.

Next chapters will cover some of the new things you are seeing in the converted MainActivity.
Once you understand the slight differences between Java and Kotlin, you’ll be able to create new
code by yourself much easier.


[Kotlin for Android Developers repository]: https://github.com/antoniolg/Kotlin-for-Android-Developers


