# Simplified setOnClickListener()

We use the very typical example of Android to explain how it works: `View.setOnClickListener()` method. If we want to use the Java way to increase the call event callback, I first have to write a `OnClickListener` interface:

```java
public interface OnClickListener {
    void onClick(View v);
}
```

Then we have to write an anonymous inner class to implement this interface:

```java
view.setOnClickListener(new OnClickListener(){
	@Override
	public void onClick(View v) {
		Toast.makeText(v.getContext(), "Click", Toast.LENGTH_SHORT).show();
	}
})
```

We will convert the above code into Kotlin (using Anko's toast function):

```kotlin
view.setOnClickListener(object : OnClickListener {
	override fun onClick(v: View) {
		toast("Click")
	}
}
```

Fortunately, Kotlin allows some optimization of the Java library, which contains a single function that can be replaced by a function. If we are so defined, it will normally execute:

```kotlin
fun setOnClickListener(listener: (View) -> Unit)
```

一A lambda expression is defined by the argument on the left side of the arrow (surrounded by parentheses) and then returns the resulting value to the right of the arrow. In this example, we receive a View and then return a Unit (nothing). So according to this idea, we can simplify the previous code into this:

```kotlin
view.setOnClickListener({ view -> toast("Click")})
```

It is great to simplify! When we define a method, we must enclose it with curly braces, and then specify the parameters on the left side of the arrow, and return the result of the function execution on the right side of the arrow. If the left side of the parameters are not used, we can even omit the left of the parameters:

```kotlin
view.setOnClickListener({ toast("Click") })
```

If the last argument to this function is a function, we can move the function outside the parentheses:

```kotlin
view.setOnClickListener() { toast("Click") }
```

And, finally, if the function has only one argument, we can omit the parentheses:

```kotlin
view.setOnClickListener { toast("Click") }
```

Than the original Java code is shorter than 5 times, and easier to understand what it does. Very impressive.
