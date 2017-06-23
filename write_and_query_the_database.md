# Write and query the database

`SqliteOpenHelper` is just a tool that is a channel between SQL World and OOP. We want to create several new classes to request data that has been saved in the database and save the new data. The defined class will use `ForecastDbHelper` and` DataMapper` to convert the data in the database to `domain models`. I still use the default value to achieve a simple dependency injection:

```kotlin
class ForecastDb(
    val forecastDbHelper: ForecastDbHelper = ForecastDbHelper.instance,
    val dataMapper: DbDataMapper = DbDataMapper()) {
    ...
}
```

All the functions use the `use()` function mentioned in the previous section. The value returned by lambda is also used as the return value for this function. So let's define a function that uses `zip code` and` date` to query a `forecast` :

```kotlin
fun requestForecastByZipCode(zipCode: Long, date: Long) = forecastDbHelper.use {
	...
}
```

So there is no explanation: we use the `use` function to return the results as a result of this function.

#### Query a forecast

The first query to do is the daily weather forecast, because we need this list to create a `city` object. Anko provided a simple request builder, so let's take advantage of this favorable condition:

```kotlin
val dailyRequest = "${DayForecastTable.CITY_ID} = ? " +
    "AND ${DayForecastTable.DATE} >= ?"
    
val dailyForecast = select(DayForecastTable.NAME)
        .whereSimple(dailyRequest, zipCode.toString(), date.toString())
        .parseList { DayForecast(HashMap(it)) }
```

The first line, `dailyRequest`, is part of the `where` 's in the query. It is the first parameter that the `whereSimple` function needs, which is very similar to what we do with the usual helper. There is another simplified `where` function, which requires some tags and values to match. I do not like this way, because I think this adds to the code of the template, although this is the analysis of the value of String is very beneficial to us. At last it looks like this:

```kotlin
val dailyRequest = "${DayForecastTable.CITY_ID} = {id}" + "AND ${DayForecastTable.DATE} >= {date}"

val dailyForecast = select(DayForecastTable.NAME)
        .where(dailyRequest, "id" to zipCode, "date" to date)
        .parseList { DayForecast(HashMap(it)) }
```

You can choose one of the ways you like. `Select` function is very simple, it just needs to be a lookup table name. `Parse` function when there will be some magic in it. In this example we assume that the result of the request is a list, using the `parseList` function. It uses the `RowParser` or `RapRowParser` function to convert the cursor into a collection of objects. The two differences are that `RowParser` is the order of the columns, and `MapRowParser` is the key name from the map.

There are two overloaded conflicts between them, so we can not directly create the needed objects in a simplified way. But nothing can not be solved by an extended function. I created a function that takes a lambda function to return a `MapRowParser`. The parser will call this lambda to create this object:

```kotlin
fun <T : Any> SelectQueryBuilder.parseList(
    parser: (Map<String, Any>) -> T): List<T> =
        parseList(object : MapRowParser<T> {
            override fun parseRow(columns: Map<String, Any>): T = parser(columns)
})
```

This function can help us simply go to the `parseList` query results:

```kotlin
parseList { DayForecast(HashMap(it)) }
```

The `immortable map` received by the parser is transformed into a `mutable map` (which we can modify in `database model`) by using the corresponding `HashMap` constructor. The `HashMap` will be used in the constructor in `DayForecast`.

So, this query returns a `Cursor`, to understand what is happening behind this scene. `ParseList` will iterate over it, then get every row of `Cursor` until the last one. For each row, it creates a map that contains the key for this column and the assignment to the corresponding key. And then return this map to the parser.

If the query does not have any results, `parseList` will return an empty list.

The next step to query the city is the same way:

```kotlin
val city = select(CityForecastTable.NAME)
        .whereSimple("${CityForecastTable.ID} = ?", zipCode.toString())
        .parseOpt { CityForecast(HashMap(it), dailyForecast) }
```

The difference is that we are using `parseOpt` . This function returns a nullable object. The result can be a null or a single object, depending on whether the request can query the data in the database. There is another function called `parseSingle`, which is essentially the same, but it returns a non-nullable object. So if it does not find this data in the database, it will throw an exception. In our example, the first time you query a city, it certainly does not exist, so using `parseOpt` will be safer. I also created a nice function to prevent the creation of the objects we need:

```kotlin
public fun <T : Any> SelectQueryBuilder.parseOpt(
    parser: (Map<String, Any>) -> T): T? =
        parseOpt(object : MapRowParser<T> {
            override fun parseRow(columns: Map<String, Any>): T = parser(columns)
        })
```

Finally, if the returned city is not null, we use `dataMapper` to convert it to `domain object` and return it. Otherwise, we return null directly. You should remember that the last line of lambda represents the return value. So this will return a `CityForecast?` Type of object:

```kotlin
if (city != null) dataMapper.convertToDomain(city) else null
```

The `DataMapper` function is simple:

```kotlin
fun convertToDomain(forecast: CityForecast) = with(forecast) {
    val daily = dailyForecast.map { convertDayToDomain(it) }
    ForecastList(_id, city, country, daily)
}

private fun convertDayToDomain(dayForecast: DayForecast) = with(dayForecast) {
	Forecast(date, description, high, low, iconUrl)
}
```

The last complete function is as follows:

```kotlin
fun requestForecastByZipCode(zipCode: Long, date: Long) = forecastDbHelper.use {

	val dailyRequest = "${DayForecastTable.CITY_ID} = ? AND " +
	    "${DayForecastTable.DATE} >= ?"
	val dailyForecast = select(DayForecastTable.NAME)
	        .whereSimple(dailyRequest, zipCode.toString(), date.toString())
	        .parseList { DayForecast(HashMap(it)) }
	        
	val city = select(CityForecastTable.NAME)
	        .whereSimple("${CityForecastTable.ID} = ?", zipCode.toString())
	        .parseOpt { CityForecast(HashMap(it), dailyForecast) }

    if (city != null) dataMapper.convertToDomain(city) else null
}
```

Another funny function of Anko is here to show that you can use `classParser()` instead of `MapRowParser`, which is based on the column name to generate objects by reflection. I like another way because I do not need to use reflection and have control over the conversion, but sometimes it's useful for you.

#### Save a forecast

The `saveForecast` function simply clears the data from the database, then converts the `domain` model to the database model, and then inserts the `forecast` and `city forecast` 'for each day. This structure is simpler than before: it returns the data from `database helper` through the `use` function. In this example we do not need to return the value, so it will return `Unit`.

```kotlin
fun saveForecast(forecast: ForecastList) = forecastDbHelper.use {
    ...
}
```

First, we empty the two tables. Anko did not offer a pretty way to do this, but that did not mean we could not. So we created an extension function of `SQLiteDatabase` so that we can execute it like a SQL query:

```kotlin
fun SQLiteDatabase.clear(tableName: String) {
	execSQL("delete from $tableName")
}
```

Empty the two tables:

```kotlin
clear(CityForecastTable.NAME)
clear(DayForecastTable.NAME)
```

Now, it is time to convert the implementation of `insert` after the return of the data. At this point you may be up to the fans of my `with` function:

```kotlin
with(dataMapper.convertFromDomain(forecast)) {
	...
}
```

The way to convert from `domain model` is also straightforward:

```kotlin
fun convertFromDomain(forecast: ForecastList) = with(forecast) {
    val daily = dailyForecast.map { convertDayFromDomain(id, it) }
    CityForecast(id, city, country, daily)
}

private fun convertDayFromDomain(cityId: Long, forecast: Forecast) =
    with(forecast) {
        DayForecast(date, description, high, low, iconUrl, cityId)
	}
```

In the code block, we can use `dailyForecast` and` map` without using references and variables, just as we do in this class. For the insert we use another Anko function, it takes a table name and a `vararg` modified `Pair<String, Any>` as a parameter. This function converts `vararg` to the `ContentValues` object needed by the Android SDK. So our task is to convert `map` into a `vararg` array. We created an extension function for `MutableMap`:

```kotlin
fun <K, V : Any> MutableMap<K, V?>.toVarargArray():
    Array<out Pair<K, V>> =  map({ Pair(it.key, it.value!!) }).toTypedArray()
```

It supports the null values (this is the condition `map delegate`), convert it to a non-null value `Pairs` ( `select` function needed) `Array` thereof. Do not worry even if you do not fully understand this function, I will soon talk about the nullability.

So, this new function we can use:

```kotlin
insert(CityForecastTable.NAME, *map.toVarargArray())
```

It inserted a new line of data in the `CityForecast`. Expressed using this array is broken down into a `vararg` front` toVarargArray` parameter function results. This is handled automatically in Java, but we need to specify in Kotlin.

The weather forecast is the same every day:

```kotlin
dailyForecast.forEach { insert(DayForecastTable.NAME, *it.map.toVarargArray()) }
```

So, by using the `map`, we can convert the class to a data table in a very simple way, and vice versa. Because we have created a new extension function, we can use in other projects, this is really valuable place.

The complete code for this function is as follows:

```kotlin
fun saveForecast(forecast: ForecastList) = forecastDbHelper.use {
	clear(CityForecastTable.NAME)
	clear(DayForecastTable.NAME)
	
	with(dataMapper.convertFromDomain(forecast)) {
	    insert(CityForecastTable.NAME, *map.toVarargArray())
	    dailyForecast forEach {
	        insert(DayForecastTable.NAME, *it.map.toVarargArray())
	    }
	}
}
```

There are a lot of code needed in this chapter, so you can check out the checkout in the code base.
