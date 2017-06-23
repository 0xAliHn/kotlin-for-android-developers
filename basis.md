# basis

For example, we can create a specified generic class:

```kotlin
class TypedClass<T>(parameter: T) {
    val value: T = parameter
}
```

This class can now be initialized using any type, and the parameter will also use the defined type, we can do this:

```kotlin
val t1 = TypedClass<String>("Hello World!")
val t2 = TypedClass<Int>(25)
```

But Kotlin is simple and downsets the template code, so if the compiler can infer the type of the argument, we do not even need to specify it:

```kotlin
val t1 = TypedClass("Hello World!")
val t2 = TypedClass(25)
val t3 = TypedClass<String?>(null)
```

If the third object receives a null reference, it still needs to specify its type because it can not be inferred.

We can add the type restriction in the way that is specified in Java as defined in Java. For example, if we want to restrict the non-null type in the previous class, we only need to do this:

```kotlin
class TypedClass<T : Any>(parameter: T) { 
	val value: T = parameter
}
```

If you go to compile the previous code, you will see that `t3` will now throw an error. The nullable type is no longer allowed. But the restrictions can be more severe. What if we only want the subclasses of `Context` to do? Very simple:

```kotlin
class TypedClass<T : Context>(parameter: T) { 
	val value: T = parameter
}
```

Now all classes that inherit `Context` can be used in our class. Other types are not allowed.

Of course, you can use the function. We can build generic functions fairly simply:

```kotlin
fun <T> typedFunction(item: T): List<T> {
	...
}
```
