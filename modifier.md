# Modifier

### private

The `private` modifier is the most restrictive modifier we use. It means that it can only be seen by the file where it is. So if we declare a class as `private`, we can not use it in a file that defines this class.

On the other hand, if we use the `private` modifier in a class, that access is restricted to this class. Even subclasses that inherit this class can not use it.

So first-class citizens, classes, objects, interfaces ... (ie, package members) are defined as `private`, so they will only be visible to the file where the definition is located. If they are defined in a class or interface, they are only visible to this class or interface.

### protected

This modifier can only be used in class or interface members. A package member can not be defined as `protected`. Defined in a member, the same way as in Java: it can be seen by its own members and members that inherit it (for example, the class and its subclass).

### internal

If it is a definition of `internal` package members, then the entire` module` visible. If it is a member of another area, it will depend on the visibility of that area. For example, if we write a `private` class, then the visibility of its` internal` modifier function will limit the visibility of the class with which it is located.

We can access the same `module` in the` internal` modified class, but can not access the other `module`.

> What is `module`
>> According to Jetbrains definition, a `module` should be a separate functional unit, it should be able to be compiled separately, run, test, debug. Depending on the modules of our project, you can create different `module`s in Android Studio. In Eclipse, these `module`s can be considered in a `workspace` 's different `project`.

### public

You should probably think that this is the least restrictive modifier. This is the default __modifier__, members are modified to `public` at any place, it is clear that it is only limited to its fields. A member defined as `public` is included in a` private` decorated class, and this member is not visible outside of this class.
