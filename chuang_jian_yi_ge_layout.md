# Create a layout

Show the list of weather forecasts We use `RecyclerView`, so you need to add a new dependency to` build.gradle`:

```groovy
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile "com.android.support:appcompat-v7:$support_version" 
    compile "com.android.support:recyclerview-v7:$support_version" ...
}
```

Then, `activity_main.xml` reads:

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

In `Mainactivity.kt` to remove all the code used to test before it can run properly (should now be prompted to error). For the time being we use the old `findViewByid()` way:

```kotlin
val forecastList = findViewById(R.id.forecast_list) as RecyclerView
forecastList.layoutManager = LinearLayoutManager(this)
```

As you can see, we define a class variable and transform it into `RecyclerView`. This is a bit different from Java, and we'll analyze the differences in the next chapter. `LayoutManager` will be set by the property, rather than through the setter, this layout is enough to display a list.

>Object instantiation
>>Object instantiation is also somewhat different from Java. As you can see, we removed the `new` keyword. At this point the constructor will still be called, but we omit the valuable four characters. `LinearLayoutManager(this)` Creates an instance of an object.
