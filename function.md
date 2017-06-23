# Function

Function (our Java method) can use the `fun` keyword to define:

```kotlin
fun onCreate(savedInstanceState: Bundle?) {
}
```

If you do not specify its return value, it will return `Unit`, similar to` void` in Java, but `Unit` is a real object. You can of course also specify any other return type:

```kotlin
fun add(x: Int, y: Int) : Int {
    return x + y
}
```

>Tip: semicolon is not required
>>I think you saw in the above example, I did not use the semicolon at the end of each sentence. Of course you can also use semicolons, semicolons are not required, and do not use semicolons is a good practice. When you do that, you will find that it saves you a lot of time.

However, if the returned result can be calculated using an expression, you can use the equal sign instead of using parentheses:

```kotlin
fun add(x: Int,y: Int) : Int = x + y
```
