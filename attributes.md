# Attributes

Attributes are the same as those in Java, but are more powerful. Attributes do things with fields plus getter plus setter. We compare their differences through an example. This is the code required for field security access and modification in Java:

```java
public class Person {
    private String name;
    public String getName() {
        return name;
    }
    public void setName(String name) { 
        this.name = name;
    }
}
...
Person person = new Person();
person.setName("name");
String name = person.getName();
```

In Kotlin, only one attribute is needed:

```kotlin
public class Person {
    var name: String = ""
}

...

val person = Person()
person.name = "name"
val name = person.name
```

If there is no specification, the property will use getter and setter by default. Of course it can also be modified for your custom code and does not modify the existing code:

```kotlin
public classs Person {
    var name: String = ""
        get() = field.toUpperCase()
        set(value){
            field = "Name: $value"
        }
}
```

If you need to access the value of the property itself in the getter and setter, it needs to create a `backing field`. You can use the `field` this reserved field to access, it will be found by the compiler is being used and automatically created. It is important to note that if we call the attribute directly, we will use setter and getter instead of directly accessing the property. `Backing field` can only be accessed within the property accessor.

As mentioned in the previous section, when operating Java code, Kotlin will allow the use of the syntax of the property to access the getter/setter method defined in the Java file. The compiler will link directly to its original getter/setter method. So when we directly access the property when there will be no performance overhead.
