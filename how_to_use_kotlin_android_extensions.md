# How to use Kotlin Android Extensions

If you still remember, now the project is ready to use Kotlin Android Extensions. When we created this project, we have already added this dependency to `build.gradle`:

```groovy
buldscript{
	repositories {
		jcenter()
	}
	dependencies {
		classpath "org.jetbrains.kotlin:kotlin-android-extensions:$kotlin_version"
	}
}
```

The only thing that needs this plugin to do is add a specific "manual" `import` in the class to use this feature. We have two ways to use it：

#### `Activities` or` Fragments` 's Android Extensions

This is the most typical way to use it. They can be accessed as properties of `activity` or` fragment`. The name of the attribute is the id of the view in XML.

We need to use the `import` statement to start with `kotlin.android.synthetic` and then add the name of the XML we want to bind to the activity XML:

```kotlin
import kotlinx.android.synthetic.activity_main.*
```

After that, we can access these views after `setContentView` is called. The new Android Studio version can add an embedded layout to the Activity default layout by using the `include` tag. It is important to note that for these layouts, we also need to add manual import:

```kotlin
import kotlinx.android.synthetic.activity_main.*
import kotlinx.android.synthetic.content_main.*
```

#### `Views` of` Android Extensions`

The use of the front is still limited, because there may be a lot of code need to visit the view in XML. For example, a custom view or an adapter. For example, bind a view in xml to another view. The only difference is the need for `import`:

```kotlin
import kotlinx.android.synthetic.view_item.view.*
```

If we need an adapter, for example, we now want to access properties from the inflater's View:

```kotlin
view.textView.text = "Hello"
```
