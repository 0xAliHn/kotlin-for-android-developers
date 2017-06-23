# when expression

`when` expression is similar to` switch/case` in Java, but much more powerful. This expression will try to match all possible branches until a satisfactory one is found. Then it will run the expression on the right. Unlike Java's `switch/case` ', the argument can be of any type, and the branch can also be a condition.

For the default option, we can add a `else` branch, which will be executed before any condition matches. The code that executes after a successful match can also be a block of code:

```kotlin
when (x){
	1 -> print("x == 1") 
	2 -> print("x == 2") 
	else -> {
		print("I'm a block")
		print("x is neither 1 nor 2")
    }
}
```

Because it is an expression, it can also return a value. We need to consider when to use as an expression, it must cover the possibility of all branches or implement the `else` branch. Otherwise it will not be compiled successfully:

```kotlin
val result = when (x) {
    0, 1 -> "binary"
	else -> "error"
}
```

As you can see, the condition can be a series of comma-separated values. But it can be more matching. For example, we can detect the parameter type and make judgments:

```kotlin
when(view) {
    is TextView -> view.setText("I'm a TextView")
    is EditText -> toast("EditText value: ${view.getText()}")
    is ViewGroup -> toast("Number of children: ${view.getChildCount()} ")
    else -> view.visibility = View.GONE
}
```

In the code on the right side, the parameters are automatically converted, so you do not need to explicitly do the type conversion.

It also makes it possible to detect the parameter no in an array range or even a collection range (I will speak this later in this chapter):

```kotlin
val cost = when(x) {
	in 1..10 -> "cheap"
	in 10..100 -> "regular"
	in 100..1000 -> "expensive"
	in specialValues -> "special value!"
	else -> "not rated"
}
```

Or you can even get rid of the almost crazy check of the parameters needed. It can be replaced with a simple `if/else` chain:

```kotlin
valres=when{
	x in 1..10 -> "cheap"
	s.contains("hello") -> "it's a welcome!"
	v is ViewGroup -> "child count: ${v.getChildCount()}"
	else -> ""
}
```
