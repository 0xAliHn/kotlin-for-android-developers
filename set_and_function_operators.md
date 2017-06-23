# Set and function operators

We have used the collection in our project, but it is time to show how powerful they are after combining the function operators. On the functional programming is very good that we do not have to explain how we do, but directly that I want to do. For example, if I want to filter a list, do not have to create a list, traverse each of the list, and then if you meet certain conditions into a new collection, but directly eat the filer function and indicate that I want to use Of the filter. In this way, we can save a lot of code.

Although we can directly use the collection in Java, but Kotlin also provides some of the local interface you want to use:

- __Iterable__: parent class. All we can traverse a series of are to achieve this interface.
- __MutableIterable__: A support for traversal can also be performed while deleting the Iterables.
- __Collection__: This class is a collection of norm. We can access through the function can return the size of the collection, whether it is empty, whether it contains one or some items. All the methods of this collection provide queries because connections are unmodifiable.
- __MutableCollection__: A collection that supports adding and deleting items. It provides additional functions, such as `add`, `remove`, `clear` and so on.
- __List__: probably the most popular collection type. It is a normative and orderly collection. Because of its order, we can use the `get` function to access through the position.
- __MutableList__: A List that supports adding and deleting items.
- __Set__: A disordered collection that does not support duplicate items.
- __MutableSet__: A set that supports adding and deleting items.
- __Map__: a collection of key-value pairs. Key is unique in the map, that is to say there can not be two pairs of keys is the same key value exists in a map.
- __MutableMap__: A map that supports adding and deleting items.

There are many different sets of available function operators. I want to show you through an example. It is useful to know which optional operators are used, because it makes it easier to tell when they are used.
