# What is Kotlin?

Kotlin, as mentioned earlier, is a JVM-based language developed by [JetBrains]. a company known for the creation of IntelliJ IDEA, a powerful IDE for Java development. Android Studio, the official Android IDE, is based on IntelliJ.

Kotlin was created with Java developers in mind, and with IntelliJ as its main development IDE.And these are two very interesting features for Android developers:
- Kotlin is very intuitive and easy to learn for Java developers. Most parts of the language are very similar to what we already know, and the differences in basic concepts can be learn in no time.
- We have total integration with our daily IDE for free. Android Studio can understand, compile and run Kotlin code. And the support for this language comes from the company who develops the IDE, so we Android developers are first-class citizens.
 
 
But this is only related to how the language integrates with our tools. What are the advantages of the language when compared to Java 7?
- It’s more expressive: this is one of its most important qualities. You can write more with much less code.
- It’s safer: Kotlin is null safe, which means that we deal with possible null situations in compile
time, to prevent execution time exceptions. We need to explicitly specify if an object can be
null, and then check its nullity before using it. You will save a lot of time debugging null
pointer exception and fixing nullity bugs.
- It’s functional: Kotlin is basically an object oriented language, not a pure functional language.
However, as many other modern languages, it uses many concepts from functional programming,
such as lambda expressions, to resolve some problems in a much easier way. Another
nice feature is the way it deals with collections.
- It makes use of extension functions: This means we can extend any class with new features
even if we don’t have access to the its source code.
- It’s highly interoperable: You can continue using most libraries and code written in Java,
because the interoperability between both languages is excellent. It’s even possible to create
mixed project, with both Kotlin and Java files coexisting.


[JetBrains]: https://www.jetbrains.com/
