# Function

Functions (our methods in Java) are declared just using the `fun` keyword:

```kotlin
fun onCreate(savedInstanceState: Bundle?) {
}
```

It you don’t specify a return value, it will return `Unit`, similar to `void` in Java, though this is really
an object. You can, of course, specify any type as a return value:

```kotlin
fun add(x: Int, y: Int) : Int {
    return x + y
}
```

>Tip: semicolon is not required
>>As you can see in the example above, I’m not using semi-colons at the end of the sentences.
While you can use them, semi-colons are not necessary and it’s a good practice not to use
them. When you get used, you’ll find that it saves you a lot of time.

However, if the result can be calculated using a single expression, you can get rid of brackets and
use equal:

```kotlin
fun add(x: Int,y: Int) : Int = x + y
```
