# Define tables

Create a few `objects`s so that we can avoid table name spelling errors, repeat and so on. We need two tables: one to save the city's information and the other to keep the weather forecast for the day. The second table will have a field associated with the first table.

`CityForecastTable` provides the name of the table and there is a need for an id (the city's zipCode), the name of the city and the country of the country.

```kotlin
object CityForecastTable {
    val NAME = "CityForecast"
    val ID = "_id"
    val CITY = "city"
    val COUNTRY = "country"
}
```

`DayForecast` has more information, just as you see a lot of columns below. The last column `cityId`, used to keep belongs to the city id.

```kotlin
object DayForecastTable {
	val NAME = "DayForecast"
	val ID = "_id"
	val DATE = "date"
	val DESCRIPTION = "description"
	val HIGH = "high"
	val LOW = "low"
	val ICON_URL = "iconUrl"
	val CITY_ID = "cityId"
}
```
