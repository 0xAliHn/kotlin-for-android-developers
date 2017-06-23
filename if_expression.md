# If expression

In Kotlin everything is an expression, that is to say everything returns a value. If the `if` condition does not contain an exception, then we can use it as we usually do:

```kotlin
if(x>0){
	toast("x is greater than 0")
}else if(x==0){ 
	toast("x equals 0")
}else{
	toast("x is smaller than 0")
}
```

We can also assign the result to a variable. We used it many times in our code:

```kotlin
val res = if (x != null && x.size() >= days) x else null
```

This also means that I do not need to have a ternary operator like Java, because we can use it for simple implementation:

```kotlin
val z = if (condition) x else y
```

So the `if` expression always returns a value. If a branch returns a Unit, the entire expression will also return to Unit, which can be ignored. In this case, its usage is the same as the .
