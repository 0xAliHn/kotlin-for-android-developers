# How the nullable type works

Most of the modern languages use certain methods to solve this problem, Kotlin's method is similar to other languages is quite different and different. But the gold rule is the same: if the variable can be null, the compiler forces us to do it in some way.

Specifying a variable is nullable by __adding a question mark__ at the end of the type. Because everything in Kotlin is an object (even the original data type in Java), everything is nullable. So, of course we can have a nullable integer:

```kotlin
val a: Int? = null
```

一A nul type, you can not use it directly before you check it out. This code can not be compiled:

```kotlin
val a: Int? = null
a.toString()
```

The previous line of code is marked as null, and the compiler will know it, so you can not use it before you check it. There is also a feature that when we check the nullability of an object, the object is automatically transformed into a non-nullable type, which is the smart conversion of the Kotlin compiler:

```kotlin
vala:Int?=null
...
if(a!=null){
	a.toString()
}
```

In `if`, `a` changes from `Int?` To `Int`, so we can use it without having to check it again. `If` product code, of course, we have to check processing. This is only valid if the variable can not be changed at this time, because otherwise the value may be modified by another thread, and the previous check will return false. `val` attribute or local (`val` or `var`) variable.

It sounds like it will make things more. Do we have to write a lot of code to carry out the checkability can be made? Of course not, first of all, because most of the time you do not need to use null type. Null references are not useful in our imagination, and you will find this when you want to figure out whether a variable can be null. But Kotlin also has its own solution to make it more concise. For example, we simplified the code as follows:

```kotlin
val a: Int? = null
...
a?.toString()
```

Here we use the secure access operator (`?`). Only when this variable is not null will go to the implementation of the previous line of code. Otherwise it will not do anything. And we can even use __Elvis operator__ (`?:`):

```kotlin
val a:Int? = null
val myString = a?.toString() ?: ""
```

Since `throw` and` return` are expressions in Kotlin, they can be used to the right of the __Elvis operator__ operator:

```kotlin
val myString = a?.toString() ?: return false

val myString = a?.toString() ?: throw IllegalStateException()
```

Then we may encounter this scenario, we make sure we are using a non-null variable, but this type is nullable. We can use the `!!` Operator to force the compiler may skip execution restriction checking null type:

```kotlin
val a: Int? = null
a!!.toString()
```

The above code will be compiled, but it will obviously crash. So we want to make sure we can only use it in certain circumstances. Usually we can choose ourselves as a solution. If a piece of code that is so full of `!!`, it smelled of code that is badly handled the.
