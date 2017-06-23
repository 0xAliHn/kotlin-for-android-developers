# Data class

The data class is a very powerful class that allows you to avoid creating template code in Java that saves state but works very simply with POJO. They usually only provide simple getters and setters for accessing their properties. Defining a new data class is very simple:

```kotlin
data class Forecast(val date: Date, val temperature: Float, val details: String)
```
