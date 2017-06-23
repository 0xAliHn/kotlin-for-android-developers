# Construction methods and function parameters

The parameters in Kotlin are somewhat different from those in Java. As you can see, we write the name of the parameter and write it to the type:

```kotlin
fun add(x: Int, y: Int) : Int {
    return x + y
}
```

We can give the parameters to specify a default value so that they become optional, which is very helpful. Here is an example of creating a function in the Activity to toast a piece of information:

```kotlin
fun toast(message: String, length: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, message, length).show()
}
```

As you can see, the second argument `length` specifies a default value. This means that when you call the second value can be passed or not pass, so you can avoid the need to overload the function:

```kotlin
toast("Hello")
toast("Hello", Toast.LENGTH_LONG)
```

This is the same as the following Java code:

```java
void toast(String message){
}

void toast(String message, int length){
    Toast.makeText(this, message, length).show();
}
```

It's as complicated as you think. Look at this example:

```kotlin
fun niceToast(message: String,
                tag: String = javaClass<MainActivity>().getSimpleName(),
                length: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, "[$className] $message", length).show()
}
```

I added the third default value to the tag name of the class name. If the total overhead in Java will grow geometrically. It can now be called by:

```kotlin
toast("Hello")
toast("Hello", "MyTag")
toast("Hello", "MyTag", Toast.LENGTH_SHORT)
```

And even other options, because you can use the parameter name to call, which means that you can pass the value of the parameters before the name of the parameters you want to pass:

```kotlin
toast(message = "Hello", length = Toast.LENGTH_SHORT)
```

>Tip: String template
>> You can use template expressions directly in String. It can help you to simply write complex strings on static values and variables. In the above example, I used "[$className] $message".

>> As you can see, any time you use a `$` sign can insert an expression. If this expression is a bit complicated, you need to use a pair of braces to enclose: "Your name is ${user.name}".



