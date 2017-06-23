# Dependent injection

I tried not to add very complicated structural code, keep the code of the testability and good practice, and I think I should use Kotlin to simplify the code from other aspects. If you want to learn about some of the reverse or dependency injection topics, you can check out a series of articles about [using Dagger in Android]. The first article has a brief description of their team.

A simple way, if we want to have some classes that are independent of other classes, make it easier to test and write code that is easy to extend and maintain when we need to use dependency injection. We do not instantiate in the class, we provide them in other places (usually through the constructor) or instantiate them. In this way, we can use other objects to replace them. For example, you can achieve the same interface or in the use of mocks tests.

But now that these dependencies must be provided in some places, dependency injection consists of classes that provide partners. These are usually done using a dependency injector. [Dagger] is probably the most popular dependency injector on Android. Of course, when we provide a certain degree of complexity is a good alternative.

But the smallest alternative is to use the default value in this constructor. We can provide a dependency to the constructor's parameters by assigning default values, and then provide different instances in different situations. For example, in our `ForecastDbHelper` we can use a more intelligent way to provide a context:

```kotlin
class ForecastDbHelper(ctx: Context = App.instance) :
        ManagedSQLiteOpenHelper(ctx, ForecastDbHelper.DB_NAME, null,
        ForecastDbHelper.DB_VERSION) {
        ...
}
```

Now we have two ways to create this class:

```kotlin
val dbHelper1 = ForecastDbHelper() // 它会使用 App.instance
val dbHelper2 = ForecastDbHelper(mockedContext) // 比如，提供给测试tests
```

I will use this feature everywhere, so I will continue to explain after explaining it clearly. We already have tables, so it's time to start adding and asking for them. But before that, I would like to talk about the combination and function operators. Do not forget to check the codebase to find the latest code.

[using Dagger in Android]: http://http://antonioleiva.com/dependency-injection-android-dagger-part-1/
[Dagger]: http://square.github.io/dagger/
