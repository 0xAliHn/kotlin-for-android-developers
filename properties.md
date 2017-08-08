# Properties

Properties are the equivalent to fields in Java, but much more powerful. Properties will do the work
of a field plus a getter plus a setter. Let’s see an example to compare the difference. This is the code
required in Java to safely access and modify a field:

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

In Kotlin, only a property is required:

```kotlin
public class Person {
    var name: String = ""
}

...

val person = Person()
person.name = "name"
val name = person.name
```

If nothing is specified, the property uses the default getter and setter. It can, of course, be modified
to run whatever custom code you need, without having to change the existing code:

```kotlin
public classs Person {
    var name: String = ""
        get() = field.toUpperCase()
        set(value){
            field = "Name: $value"
        }
}
```

If the property needs access to its own value in custom getter and setter (as in this case), it requires
the creation of a `backing field`. It can be accessed by using `field` a reserved word, and will be
automatically created by the compiler when it finds that it’s being used. Take into account that
if we used the property directly, we would be using the setter and getter, and not doing a direct
assignment. The `backing field` can only be used inside property accessors.

As mentioned in some previous chapters, when operating with Java code Kotlin will allow to use
the property syntax where a getter and a setter are defined in Java. The compiler will just link to
the original getters and setters, so there are no performance penalties when using these mapped
properties.
