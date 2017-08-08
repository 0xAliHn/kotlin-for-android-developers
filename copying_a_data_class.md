# Copying a data class

If we use immutability, as talked some chapters ago, we’ll find that if we want to change the state of
an object, a new instance of the class is required, with one or more of its properties modified. This
task can be rather repetitive and far from clean. However, data classes include the `copy()` method,
which will make the process really easy and intuitive.

For instance, if we need to modify the temperature of a `Forecast` we can just do:

```kotlin
val f1 = Forecast(Date(), 27.5f, "Shiny day")
val f2 = f1.copy(temperature = 30f)
```

This way, we copy the first forecast and modify only the  `temperature` property without changing
the state of the original object.

> Be careful with immutability when using Java classes
> > If you decide to work with immutability, be aware that Java classes weren’t designed with
this in mind, and there are still some situations where you will be able to modify the state.
In the previous example, you could still access the  `Date` object and change its value. The
easy (and unsafe) option is to remember the rules of not modifying the state of any object,
but copying it when necessary.

>> Another option is to wrap these classes. You could create an `ImmutableDate` class which wraps a  
`Date` and doesn’t allow to modify its state. It’s up to you to decide which solution
you take. In this book, I won’t be very strict with immutability (as it’s not its main goal),
so I won’t create wrappers for every potentially dangerous classes.
