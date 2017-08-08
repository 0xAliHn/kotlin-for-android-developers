# Extension functions

An extension function is a function that adds a new behaviour to a class, even if we don’t have access
to the source code of that class. It’s a way to extend classes which lack some useful functions. In Java,
this is usually implemented in utility classes which include a set of static methods. The advantage
of using extension functions in Kotlin is that we don’t need to pass the object as an argument. The
extension function acts as if it belonged to the class, and we can implement it using `this` and all its
                                                                                             public methods.

For instance, we can create a `toast` function which doesn’t ask for the context, which could be used
by any `Context` objects, and those whose type extends Context, such as `Activity` or `Service`:

```kotlin
fun Context.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, message, duration).show()
}
```

This function can be used inside an activity, for instance:

```kotlin
toast("Hello world!")
toast("Hello world!", Toast.LENGTH_LONG)
```

OOf course, Anko already includes its own `toast` extension function, very similar to this one. Anko
provides functions for both  `CharSequence` and` resource`, and different functions for short and long
toasts:

```kotlin
toast("Hello world!")
longToast(R.id.hello_world)
```

Extension functions can also be properties. So you can create extension properties too in a very
similar way. The following example is showing a way to generate a property with its own getters
and setters. Kotlin already provides this property for us as an interoperability feature, but it’s a good
exercise to understand the idea behind extension properties:

```kotlin
public var TextView.text: CharSequence
    get() = getText()
    set(v) = setText(v)
```

Extension functions don’t really modify the original class, but the function is added as a static import
where it is used. Extension functions can be declared in any file, so a common practice is to create
files which include a set of related functions.

And this is the magic behind many Anko features. From now own, you can create your own magic
too.