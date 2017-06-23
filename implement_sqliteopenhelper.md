# Implement SqliteOpenHelper

The basic composition of our `SqliteOpenHelper` is the creation and updating of the database and the provision of a `SqliteDatebase` so that we can use it to work. The query can be extracted and placed in other classes:

```kotlin
class ForecastDbHelper() : ManagedSQLiteOpenHelper(App.instance,
        ForecastDbHelper.DB_NAME, null, ForecastDbHelper.DB_VERSION) {
	...
}
```

We used in the previous section we used to create the `App.instance`, this time we also include the database name and version. These values we will be defined with `SqliteOpenHelper` in the `companion object`:

```kotlin
companion object {
    val DB_NAME = "forecast.db"
    val DB_VERSION = 1
    val instance: ForecastDbHelper by lazy { ForecastDbHelper() }
}
```

`Instance` This property uses the `lazy` delegate, which indicates that it will not be created until it is actually called. In this way, if the database has never been used, we do not need to create this object. The generic `lazy` delegate code block can prevent multiple objects from being created in multiple different threads. This will only occur in the two threads in the colleague time to visit the `instance` object, it is difficult to happen but there are specific to see the implementation of the app. No one how, `lazy` commission is thread safe.

In order to create these defined tables, we need to provide an implementation of the `onCreate` function. When there is no library to use, the creation of the table will be through our preparation of the original contains our definition of the column and type of `CREATE TABLE` statement to achieve. However, Anko provides a simple extension function that takes a table name and a series of `Pair` objects built by column names and types:

```kotlin
db.createTable(CityForecastTable.NAME, true,
        Pair(CityForecastTable.ID, INTEGER + PRIMARY_KEY),
        Pair(CityForecastTable.CITY, TEXT),
        Pair(CityForecastTable.COUNTRY, TEXT))
```

- The first argument is the name of the table
- The second parameter, when it is true, will check whether the table exists before it is created.
- The third argument is a `vararg` parameter of the `Pair` type. `Vararg` also exists in Java, which is a function that passes in a function that links many of the same types of parameters. This function also receives an array of objects.

Anko has a special type called `SqlType`, which can be mixed with `SqlTypeModifiers`, such as `PRIMARY_KEY`. `Operator` is rewritten as before. The `plus` function will combine the two in the right way and then return a new `SqlType`:

```kotlin
fun SqlType.plus(m: SqlTypeModifier) : SqlType {
    return SqlTypeImpl(name, if (modifier == null) m.toString()
            else "$modifier $m")
}
```

As you can see, she will combine multiple modifiers.

But back to our code, we can modify it better. The Kotlin standard library contains a function called `to`, and once again, let's show the power of Kotlin. It takes the extended function of the first argument, receives another object as a parameter, assembles the two and returns a `Pair`.

```kotlin
public fun <A, B> A.to(that: B): Pair<A, B> = Pair(this, that)
```

Because a function with a function argument can be used inline, so the result is very clear:

```kotlin
val pair = object1 to object2
```

Then, apply them to the creation of the table:

```kotlin
db.createTable(CityForecastTable.NAME, true,
        CityForecastTable.ID to INTEGER + PRIMARY_KEY,
        CityForecastTable.CITY to TEXT,
        CityForecastTable.COUNTRY to TEXT)
```

That's what the whole function looks like:

```kotlin
override fun onCreate(db: SQLiteDatabase) {
    db.createTable(CityForecastTable.NAME, true,
            CityForecastTable.ID to INTEGER + PRIMARY_KEY,
            CityForecastTable.CITY to TEXT,
            CityForecastTable.COUNTRY to TEXT)
            
    db.createTable(DayForecastTable.NAME, true,
            DayForecastTable.ID to INTEGER + PRIMARY_KEY + AUTOINCREMENT,
            DayForecastTable.DATE to INTEGER,
            DayForecastTable.DESCRIPTION to TEXT,
            DayForecastTable.HIGH to INTEGER,
            DayForecastTable.LOW to INTEGER,
            DayForecastTable.ICON_URL to TEXT,
            DayForecastTable.CITY_ID to INTEGER)
}
```

We have a similar function used to delete the table. `OnUpgrade` will just delete the tables and then rebuild them. We just put our database as a cache, so this is a simple and safe way to ensure that our tables will be rebuilt as we would expect. If I have important data to keep, we need to optimize the code for `onUpgrade` to make the corresponding data transfer based on the database version.

```kotlin
override fun onUpgrade(db: SQLiteDatabase, oldVersion: Int, newVersion: Int) {
    db.dropTable(CityForecastTable.NAME, true)
    db.dropTable(DayForecastTable.NAME, true)
    onCreate(db)
}
```
