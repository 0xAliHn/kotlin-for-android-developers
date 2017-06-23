# For loop

Although you will not use the for loop too much after you have used the collection's function operator, the for loop is still useful in some cases. Provide an iterator which can act on anything above:

```kotlin
for (item in collection) {
	print(item)
}
```

If you need more typical iterations using index, we can also use `ranges` (anyway it's usually a more intelligent solution):

```kotlin
for (index in 0..viewGroup.getChildCount() - 1) {
    val view = viewGroup.getChildAt(index)
    view.visibility = View.VISIBLE
}
```

In a iteration of an array or list, a series of indexes can be used to get to the specified object, so the above method is not necessary:

```kotlin
for (i in array.indices)
	print(array[i])
```
