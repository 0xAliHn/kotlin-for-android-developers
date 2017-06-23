# Extended function

The number of extension functions is to add a new behavior on a class, even if we do not have access to this class code. This is a method that extends on a class that lacks useful functions. In Java, there are usually a lot of tools with static methods. One of the advantages of the extended function in Kotlin is that we do not need to pass the entire object as a parameter when calling the method. The extension function behaves like this class, and we can use the `this` keyword and call all the public methods.

For example, we can create a toast function, this function does not need to pass in any context, it can be any Context or its subclass calls, such as Activity or Service:

```kotlin
fun Context.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, message, duration).show()
}
```

This method can be called directly in the Activity:

```kotlin
toast("Hello world!")
toast("Hello world!", Toast.LENGTH_LONG)
```

Of course, Anko has included its own toast extension function, which is very similar to the above. Anko provides some functions for `CharSequence` and` resource`, and there are two different toast and longToast methods:

```kotlin
toast("Hello world!")
longToast(R.id.hello_world)
```

The extension function can also be a property. So we can extend the properties in a similar way. The following example shows how to generate a property using his own getter/setter. Kotlin has provided this property because of the interoperability features, but it is a good practice to understand the idea behind the extended attributes:

```kotlin
public var TextView.text: CharSequence
    get() = getText()
    set(v) = setText(v)
```

The extension function does not really modify the original class, it is a static way to achieve the import. The extension function can be declared in any file, so there is a common practice to put a series of related functions in a new file.

This is the magic behind the Anko function. Now through the above, you can also create your own magic.
