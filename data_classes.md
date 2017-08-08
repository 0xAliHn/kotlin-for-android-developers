# Data classes

Data classes are a powerful kind of classes which avoid the boilerplate we need in Java to create
POJO: classes which are used to keep state, but are very simple in the operations they do. They
usually only provide plain getters and setters to access to their fields. Defining a new data class is
very easy:

```kotlin
data class Forecast(val date: Date, val temperature: Float, val details: String)
```
