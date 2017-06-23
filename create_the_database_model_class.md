# Create the database model class

But first of all, we have to create a model class for the database. Do you remember the way we saw the map delegate before We have to map these properties directly to the database, and vice versa.

We first look under the `CityForecast class`:

```kotlin
class CityForecast(val map: MutableMap<String, Any?>,
                           val dailyForecast: List<DayForecast>) {
    var _id: Long by map
    var city: String by map
    var country: String by map
    
    constructor(id: Long, city: String, country: String,
                dailyForecast: List<DayForecast>)
    : this(HashMap(), dailyForecast) {
        this._id = id
        this.city = city
        this.country = country
    }
}
```

The default constructor gets a map containing the attributes and the corresponding values, and a dailyForecast. Thanks to the delegate, these values ​​are mapped to the corresponding attributes according to the name of the key. If we want the mapping process to run perfectly, then the name of the property must be exactly the same as the name in the database. We will talk about the reasons behind.

However, the second constructor is also necessary. This is because we need to map from the domain to the database class, it can not use the map, set the value from the property is also convenient. We pass in an empty map, but again, thanks to the delegate, when we set the value to the property, it will automatically add all the values ​​to the map. In this way, we are ready to map to the database. Using these useful code, I will see it running as magical as magic.

Now we need the second class, DayForecast, it will be the second table. It contains every column in the table as its property, and it also has a second constructor. The only difference is that you do not need to set id because it will grow from SQLite.

```kotlin
class DayForecast(var map: MutableMap<String, Any?>) {
	var _id: Long by map
	var date: Long by map
	var description: String by map
	var high: Int by map
	var low: Int by map
	var iconUrl: String by map
	var cityId: Long by map
	
	constructor(date: Long, description: String, high: Int, low: Int,
				iconUrl: String, cityId: Long)
	: this(HashMap()) {
		this.date = date
		this.description = description
		this.high = high
		this.low = low
		this.iconUrl = iconUrl
		this.cityId = cityId
	}
}
```

These classes will help us to map each SQLite table with each other.
