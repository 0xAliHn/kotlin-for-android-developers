# Install the Kotlin plugin

~~IDE it does not understand Kotlin itself. As mentioned in the previous section, it is designed for Java development. But the Kotlin team created a series of powerful plug-ins to make it easier for us to achieve. Go to the `Plugin` column in` Preferences` in Android Studio and install the following two plugins:~~
- ~~Kotlin: This is a basic plugin. It allows Android Studio to understand Kotlin code. It will publish a new plugin version every time the new Kotlin language version is released, so that we can discover new version features and discarded warnings through it. This is the only plugin you want to use Kotlin to write Android apps. But we still need another one now.~~
- ~~Kotlin Android Extensions: Kotlin team for Android development also released another interesting plug-in. This Android Extensions allows you to automatically inject all View from Activity in XML. For example, you do not need to use `findViewById ()`. You will immediately get a view of the conversion from the property. You will need to install this plugin to use this feature. We will explain this in depth in the next chapter.~~

Though since Intellij 15 the plugin comes installed by default, it’s possible that your Android Studio
doesn’t. So you will need to go to the plugins section inside Android Studio Preferences, and install
the Kotlin plugin. Use the search tool if you can’t find it.

Now our environment is ready to understand the language, compile it and execute it just as
seamlessly as if we were using Java.
