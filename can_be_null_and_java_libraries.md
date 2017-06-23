# Can be null and Java libraries

Well, the previous section explains the use of Kotlin code to work perfectly. But what happens with the regular Java libraries and the Android SDK? In Java, all objects can be defined as null. So we have to deal with a lot of potential null variables that can not be null in reality. This means that our code may eventually have hundreds of `!!` operators, which is definitely not a good idea.

When we go to the Android SDK, you may see that all Java method parameters are marked as a single `!`. For example, Java in some way to get the object in the Kotlin display returns `Any!`. This means that the developer himself decides whether the variable is null or not.

Fortunately, the new version of Android began using the `@Nullable` and` @NonNull` annotations to see if the argument could be null or whether a function could return null. When we suspect that we can enter the source code to check whether we will receive a null object. My guess is that in the future, the compiler can read these annotations and then force (or at least suggest) a better way.

Now, when a Jetbrains `@Nullable` annotation (which differs from an Android comment) is annotated in a non-null variable, a warning is obtained. The relative does not happen on the `@NotNull` annotation.

So let's give an example if we create a Java test class:

```java
import org.jetbrains.annotations.Nullable;
public class NullTest {

	@Nullable
	public Object getObject(){
		return "";
	}
}
```

And then used in Kotlin：

```kotlin
val test = NullTest()
val myObject: Any = test.getObject()
```

We will find that a warning is displayed on the `getObject` function. But this is only a compiler check from now on, and it does not yet know the annotations of Android, so we may have to spend more time waiting for a smarter way. In any case, using source code annotations and some Androd SDK knowledge, we are also very difficult to make mistakes.

Such as rewriting the `onCraete` function of` Activity`, we can decide whether to make `savedInstanceState` possible null:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
}

override fun onCreate(savedInstanceState: Bundle) {
}
```

These two methods will be compiled, but the second is wrong, because an Activity is likely to receive a null bundle. Just be careful a little bit enough. When you have questions, you can then use the nullable object and then deal with the possible null. Remember, if you are using `!!`, it may be because you are sure that the object can not be null, and if so, please define it as non-null.

This flexibility is really necessary in the Java library, and as the compiler evolves, we may see a better interaction (which may be annotated), but now that the mechanism is flexible enough.
