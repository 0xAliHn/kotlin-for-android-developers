# Copy a data class

If we use unmodifiable objects, as we have said before, if we need to modify the object state, we must create a new one or more attributes to be modified instance. This task is very repetitive and not simple.

For example, if we need to modify the `temperature` in the `Forecast` , we can do this:

```kotlin
val f1 = Forecast(Date(), 27.5f, "Shiny day")
val f2 = f1.copy(temperature = 30f)
```

As above, we copied the first forecast object and then only modified the `temperature` attribute without modifying the other state of the object.

> Be careful when you use Java classes "Unmodifiable"
> > If you decide to work without modification, you need to realize that Java is not designed according to this idea, and in some cases we can still modify these states. In the previous example, you can still access the `Date` object and change its value. There is a simple (unsafe) way to remember all the objects that need to be modified as a rule and then copy the copy when necessary.

>> Another way is to encapsulate these classes. You can create an `ImmutableDate` class that encapsulates `Date` and does not allow you to modify their state. Decide which way depends on you. In this book, I will not limit the irreversibility, so I will not go for some dangerous class to create a wrapper class.
