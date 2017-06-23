# ManagedSqliteOpenHelper

Anko offers a lot of powerful SqliteOpenHelper to greatly simplify code. When we use a generic `SqliteOpenHelper`, we need to call` getReadableDatabase()` or `getWritableDatabase()` , and then we can perform our search and get the result. After that, we can not forget to call `close()`. Use `ManagedSqliteOpenHelper` we only need:

```kotlin
forecastDbHelper.use {
	...
}
```

In lambda inside, we can directly use the `SqliteDatabase` function. How does it work It's really fun to read the way the Anko function is implemented. You can learn a lot of Kotlin's knowledge from here:

```kotlin
public fun <T> use(f: SQLiteDatabase.() -> T): T {
    try {
	    return openDatabase().f()
	} finally {
	    closeDatabase()
    }
}
```

First, `use` receives an extension function of` SQLiteDatabase`. This means that we can use `this` in curly braces and are in the `SQLiteDatabase` object. This function extension can return a value, so we can do this:

```kotin
val result = forecastDbHelper.use {
    val queriedObject = ...
    queriedObject
}
```

Keep in mind that in a function, the last line represents the return value. Because T does not have any restrictions, so we can return any object. Even if we do not want to return any value on the use of `Unit`.

With `try-finall`, the `use` method ensures that the database will be shut down regardless of whether the database operation succeeds or fails.

Also, there are a lot of useful extension functions in `sqliteDatabase`, which we will use later. But now let's first define our table and implement `SqliteOopenHelper`.
