# Implement an example in our App

An interface can be used to extract generic code of similar behavior from a class. For example, we can create an interface for handling the app's toolbar. `MainActivity` and` DetailActivity` in processing these `toolbar` share similar code.

But limited, we need to make some changes, use is defined in the layout of the `toolbar`, not the standard` ActionBar`. The first thing is to inherit the theme of `NoActionBar`. Such `toolbar` not automatically include it:

```kotlin
<style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
	<item name="colorPrimary">#ff212121</item>
	<item name="colorPrimaryDark">@android:color/black</item>
</style>
```

We use the `light` theme. And then we create a `toolbar` layout, we will later use it in other layouts:

```kotlin
<android.support.v7.widget.Toolbar
	xmlns:app="http://schemas.android.com/apk/res-auto"
	xmlns:android="http://schemas.android.com/apk/res/android"
	android:id="@+id/toolbar"
	android:layout_width="match_parent"
	android:layout_height="?attr/actionBarSize"
	android:background="?attr/colorPrimary"
	app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
	app:popupTheme="@style/ThemeOverlay.AppCompat.Light"/>
```

`Toolbar` specify its own background, a (` overflow menu` instance) for their `dark` theme and a theme for` light` generate pop-up box. We now have the same theme: `light` theme and` dark` theme `Action Bar`

The next step we will modify the layout of `MainActivity`, add a toolbar:

```kotlin
<FrameLayout
	xmlns:android="http://schemas.android.com/apk/res/android"
	android:layout_width="match_parent"
	android:layout_height="match_parent">

	<android.support.v7.widget.RecyclerView
		android:id="@+id/forecastList"
		android:layout_width="match_parent"
		android:layout_height="match_parent"
		android:clipToPadding="false"
		android:paddingTop="?attr/actionBarSize"/>
	
	<include layout="@layout/toolbar"/>
</FrameLayout>
```

Now that the toolbar is added to the layout, we can start using it. We created an interface that allows us to:

- change title
- Specifies whether to display the previous navigation action
- scroll when the toolbar animation
- set the same menu for all the activities, and even behavior

Then let's define `ToolbarManager`:

```kotlin
interface ToolbarManager {
    val toolbar: Toolbar
    ...
}
```

It will need a toolbar property. The interface is stateless, so the attribute can be defined, but can not be assigned. Subclasses will implement this interface and override this property.

On the other hand, we can not use rewrite to achieve stateless properties. That is, attributes do not need to maintain a `backup field`. An example of handling the toolbar title attribute:

```kotlin
var toolbarTitle: String
    get() = toolbar.title.toString()
    set(value) {
        toolbar.title = value
    }
```

Because the property just uses the toolbar, it does not need to save any new state.

We now create a new function to initialize the toolbar, inflate a menu and set a listener:

```kotlin
fun initToolbar(){
	toolbar.inflateMenu(R.menu.menu_main)
    toolbar.setOnMenuItemClickListener {
		when (it.itemId) {
		    R.id.action_settings -> App.instance.toast("Settings")
		    else -> App.instance.toast("Unknown option")
		}
		true
	}
}
```

We can add a function to open the toolbar above the navigation icon, set an arrow icon and set an icon when the trigger is triggered by the event:

```kotlin
fun enableHomeAsUp(up: () -> Unit) {
       toolbar.navigationIcon = createUpDrawable()
       toolbar.setNavigationOnClickListener { up() }
}

private fun createUpDrawable() = with (DrawerArrowDrawable(toolbar.ctx)){
    progress = 1f
	this
}
```

This function takes a listener, uses [DrawerArrowDrawable] to create a last hop (when the arrow has been shown), and then sets the listener to the toolbar.

Finally, the interface will provide a function that allows the toolbar to be attached to a scroll above, and the animation is executed according to the direction of the scroll. When the scroll down the toolbar will disappear, scroll up the toolbar will show:

```kotlin
fun attachToScroll(recyclerView: RecyclerView) {
    recyclerView.addOnScrollListener(object : RecyclerView.OnScrollListener() {
		override fun onScrolled(recyclerView: RecyclerView?, dx: Int, dy: Int) {
			if (dy > 0) toolbar.slideExit() else toolbar.slideEnter()
	    }
    })
}
```

We will create two extension functions for viewing the view from the screen or disappearing animation. We will check whether the animation has not been implemented before. This way you can avoid each time a different scroll view will be animated:

```kotlin
fun View.slideExit() {
	if (translationY == 0f) animate().translationY(-height.toFlat())
}

fun View.slideEnter() {
	if (translationY < 0f) animate().translationY(0f)
}
```

After `toobar manager` implementation, it is time to use it in` MainActivity` '. We first specify the toolbar attribute. We can use `lazy` commission to achieve, this will be the first time we use it will be inflate:

```kotlin
override val toolbar by lazy { find<Toolbar>(R.id.toolbar) }
```

`MainActivity` will only initialize the toolbar and attach to scroll` RecyclerView` and modify the title toolbar:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) { 
super.onCreate(savedInstanceState) 
setContentView(R.layout.activity_main) initToolbar()
    forecastList.layoutManager = LinearLayoutManager(this)
    attachToScroll(forecastList)
    async {
        val result = RequestForecastCommand(94043).execute()
        uiThread {
			val adapter = ForecastListAdapter(result) {
		    startActivity<DetailActivity>(DetailActivity.ID to it.id,
			            DetailActivity.CITY_NAME to result.city)
			}
			forecastList.adapter = adapter
			toolbarTitle = "${result.city} (${result.country})"
		} 
	}
}
```

`DetailActivity` also needs some changes on the layout:

```kotlin
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    
    <include layout="@layout/toolbar"/>
    
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:gravity="center_vertical"
		android:paddingTop="@dimen/activity_vertical_margin"
        android:paddingLeft="@dimen/activity_horizontal_margin"
        android:paddingRight="@dimen/activity_horizontal_margin"
        tools:ignore="UseCompoundDrawables">
       ....
    </LinearLayout>
    
</LinearLayout>
```

Use the same way to specify the toolbar attribute. `DetailActivity` will also initialize the toolbar, set the title and turn on the navigation Return icon:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_detail)
    
	initToolbar()
	toolbarTitle = intent.getStringExtra(CITY_NAME) 
	enableHomeAsUp { onBackPressed() }
	...
}
```

The interface can help us to extract common code from the class to share similar behavior. Can be used as a refinement alternative to our code refinement. Think about which interface can help you write better code.

[DrawerArrowDrawable]: https://developer.android.com/reference/android/support/v7/graphics/drawable/DrawerArrowDrawable.html
