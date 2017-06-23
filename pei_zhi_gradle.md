# Configure Gradle

The Kotlin plugin includes a tool that lets us configure Gradle. But I still tend to keep my control of Gradle files read, otherwise it will only become chaotic and will not become simple. Anyway, it's a good idea to know how it works before using the automatic tool. So this time, we will do it manually.

First, you need to modify the parent `build.gradle` as follows:
```groovy
buildscript {
    ext.support_version = '23.1.1'
    ext.kotlin_version = '1.0.0'
    ext.anko_version = '0.8.2'
    repositories {
        jcenter()
        dependencies {
            classpath 'com.android.tools.build:gradle:1.5.0'
            classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
        }
    }
}
allprojects {
    repositories {
        jcenter() 
    }
}
```
As you can see, we created a variable to store the current version of Kotlin. You read here to check the latest version, because there may be a newer version has been released. We need to use the version number in a few different places, such as you need to add the new Kotlin plugin `dependency`. You will need to go to the Kotlin standard library again in the `build.gradle` 'of the modules you specify.

We also true for `support library` , `Anko` library is the same approach. In this way it is easier to modify all the version numbers in one place. And use the same version number, the update does not need to be modified every place.

We will add the `Kotlin` standard library, the `Anko` library, and the `Kotlin` and `Kotlin Android Extensions plugin` plugins to the dependencies.
```groovy
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'
android {
    ...
}

dependencies {
    compile "com.android.support:appcompat-v7:$support_version"
    compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
    compile "org.jetbrains.anko:anko-common:$anko_version"
}

buildscript {
    repositories {
      jcenter() 
    }
    dependencies {
      classpath "org.jetbrains.kotlin:kotlin-android-extensions:$kotlin_version"
    } 
}
```
Anko is a very powerful Kotlin library used to simplify some Android tasks. We will learn part of anko later, but now it is enough to just increase `anko-common`. The library is divided into a series of small parts so that we will not put useless parts.

