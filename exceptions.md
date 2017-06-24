# Exceptions

In Kotlin, all `Exception`s are implemented with` throwable`, containing a `message` and unchecked. This means that we will not force us to use `try / catch` anywhere. This is not the same as in Java, such as throwing the `IOException` method, we need to use` try-catch` to enclose the code block. It is not a good idea to handle the display by checking the exception. Like [Bruce Eckel], [Rod Waldhoff] or [Anders Hejlsberg] and others can give you a better view of this.

The exception is thrown in a similar way to Java:

```kotlin
throw MyException("Exception message")
```

`Try` expression is the same:

```kotlin
try{
	// Some code
}
catch (e: SomeException) {
	// deal with
}
finally {
	// Optional finally block
}
```

In Kotlin, `throw` and` try` are expressions, which means that they can be assigned to a variable. This is really useful when dealing with some border issues:

```kotlin
val s = when(x){
	is Int -> "Int instance"
	is String -> "String instance"
	else -> throw UnsupportedOperationException("Not valid type")
}
```

or

```kotlin
val s = try { x as String } catch(e: ClassCastException) { null }
```

[Bruce Eckel]: http://www.mindview.net/Etc/Discussions/CheckedExceptions
[Rod Waldhoff]: http://radio-weblogs.com/0122027/stories/2003/04/01/JavasCheckedExceptionsWereAMistake.html
[Anders Hejlsberg]: http://www.artima.com/intv/handcuffs.html
