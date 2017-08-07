# Construction methods and function parameters

Parameters in Kotlin are a bit different from Java. As you can see, we first write the name of the
parameter and then its type.

```kotlin
fun add(x: Int, y: Int) : Int {
    return x + y
}
```

An extremely useful thing about parameters is that we can make them optional by specifying a
**default value**. Here it is an example of a function you could create in an activity, which uses a toast
to show a message:

```kotlin
fun toast(message: String, length: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, message, length).show()
}
```

As you can see, the second parameter `length` specifies a default value. This means you can write
the second value or not, which avoids the need of function overloading:

```kotlin
toast("Hello")
toast("Hello", Toast.LENGTH_LONG)
```

This would be equivalent to the next code in Java:

```java
void toast(String message){
}

void toast(String message, int length){
    Toast.makeText(this, message, length).show();
}
```

And this can be as complex as you want. Check this other example:

```kotlin
fun niceToast(message: String,
                tag: String = javaClass<MainActivity>().getSimpleName(),
                length: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, "[$className] $message", length).show()
}
```

I’ve added a third parameter that includes a tag which defaults to the class name. The amount of
overloads we’d need in Java grows exponentially. You can now write these calls:

```kotlin
toast("Hello")
toast("Hello", "MyTag")
toast("Hello", "MyTag", Toast.LENGTH_SHORT)
```

And there is even another option, because named arguments can be used, which means you can
write the name of the argument preceding the value to specify which one you want:

```kotlin
toast(message = "Hello", length = Toast.LENGTH_SHORT)
```

>Tip: String template
>> You can use template expressions directly in your strings. This will make it easy to
write complex strings based on static and variable parts. In the previous example, I used "[$className] $message".

>> As you can see, anytime you want to add an expression, you need to use the `$` symbol. If
the expression is a bit more complex, you’ll need to add a couple of brackets:`"Your name
is ${user.name}"`



