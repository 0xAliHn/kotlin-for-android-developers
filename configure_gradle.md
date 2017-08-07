# Configure Gradle

The Kotlin plugin includes a tool which does the Gradle configuration for us. But I prefer to keep
control of what I’m writing in my Gradle files, otherwise they can get messy rather easily. Anyway,
it’s a good idea to know how things work before using the automatic tools, so we’ll be doing it
manually this time.

First, you need to modify the parent  `build.gradle` so that it looks like this:
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
As you can see, we are creating a variable which saves current Kotlin version. Check which version
is available when you’re reading these lines, because there’s probably a new version. We need that
version number in several places, for instance in the new dependency you need to add for the Kotlin
plugin. You’ll need it again in the module `build.gradle` where we’ll specify that this module uses
the Kotlin standard library.

We’ll do the same for the `support library`  as well as  `Anko` library. This way, it’s easier to modify all
the versions in a row, as well as adding new libraries that use the same version without having to
change the version everywhere.

We’ll add the dependencies to  `Kotlin standard library ` and `Anko` library, as well as `Kotlin` and `Kotlin Android Extensions plugin` plugins.

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
Anko is a library that uses the power of Kotlin to simplify some tasks with Android. We’ll need more
Anko parts later on, but for now it’s enough if we just add `anko-common`. This library is split into
several smaller ones so that we don’t need to include everything if we don’t use it.
