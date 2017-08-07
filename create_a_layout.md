# Create a layout

The main view that will render the forecast list will be a `RecyclerView`, so a new dependency is
required. Modify the  `build.gradle` file:

```groovy
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile "com.android.support:appcompat-v7:$support_version" 
    compile "com.android.support:recyclerview-v7:$support_version" ...
}
```

Now, in  `activity_main.xml`:

```xml
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
             android:layout_width="match_parent"
             android:layout_height="match_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/forecast_list"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>
</FrameLayout>
```

In `Mainactivity.kt` remove the line we added to test everything worked (it will be showing an
error now). We’ll continue using the good old  `findViewByid()` for the time being:

```kotlin
val forecastList = findViewById(R.id.forecast_list) as RecyclerView
forecastList.layoutManager = LinearLayoutManager(this)
```

As you can see, we define the variable and cast it to `RecyclerView`. It’s a bit different from Java, and
we’ll see those differences in the next chapter. A  `LayoutManager` is also specified, using the property
naming instead of the setter. A list will be enough for this layout, so a `LinearLayoutManager` will
make it.

>Object instantiation
>>Object instantiation presents some differences from Java too. As you can see, we omit
the  `new` word. The constructor call is still there, but we save four precious characters. `LinearLayoutManager(this)` creates an instance of the object.
