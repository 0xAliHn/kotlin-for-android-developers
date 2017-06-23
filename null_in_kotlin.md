# Null in Kotlin

If you are working with Java 7, null security is one of the most interesting features of Kotlin. But as you see in this book, it does not seem to exist, until the last chapter we almost do not need to worry about it.

We have sometimes need to define a variable package that does not contain a value by thinking about null by the [100 million dollars error] we created ourselves. In Java, although annotations and the IDE have helped us a lot in this area, we can still do this:

```kotlin
Forecast forecast = null;
forecast.toString();
```

This code can be compiled perfectly (you may get a warning from the IDE) and then execute it normally, but obviously it will throw a `NullPointerException` '. This is quite safe. And according to our idea, we should go to control everything, as the code grows, we will slowly control some of the null. So eventually get a lot of `NullPointerException` or lose a lot of null checks (maybe both).

[100 million dollars error]: https://en.wikipedia.org/wiki/Tony_Hoare
