# Create a setup activity

When the `settings` menu option for the` overflow menu` is clicked, you need to open a new Activity. So the first thing to do when you need a new `SettingActivity`:

```kotlin
class SettingsActivity : AppCompatActivity() {
	
	override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_settings)
        setSupportActionBar(toolbar)
        supportActionBar.setDisplayHomeAsUpEnabled(true)
    }

	override fun onOptionsItemSelected(item: MenuItem) = when (item.itemId) {
        android.R.id.home -> { onBackPressed(); true }
        else -> false
	}
}
```

When the user leaves the interface, we need to save the user 'preferences', so we need to handle the `Up` action like` Back` to redirect the action to `onBackPressed`. Now let's create an XML layout. For this `preference` a simple` EditText 'is enough:

```xml
<FrameLayout
	xmlns:android="http://schemas.android.com/apk/res/android"
	android:layout_width="match_parent"
	android:layout_height="match_parent">

	<include layout="@layout/toolbar"/>
	<LinearLayout
		android:orientation="vertical"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		android:layout_marginTop="?attr/actionBarSize"
		android:padding="@dimen/spacing_xlarge">
	
		<TextView
			android:layout_width="wrap_content"
			android:layout_height="wrap_content"
			android:text="@string/city_zipcode"/>
		
		<EditText
			android:id="@+id/cityCode"
			android:layout_width="match_parent"
			android:layout_height="wrap_content"
			android:hint="@string/city_zipcode"
			android:inputType="number"/>

	</LinearLayout>
</FrameLayout>
```

And then only need to declare this activity in `AndroidManifest.xml` '

```xml
<activity
android:name=".ui.activities.SettingsActivity"
android:label="@string/settings"/>
```
