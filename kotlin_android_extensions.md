# Kotlin Android Extensions

Another Kotlin team developed a new plugin that could make it easier to develop `Kotlin Android Extensions`. Currently only the binding of the view is included. This plugin automatically creates a lot of properties to let us directly access the view in XML. This way does not require you to explicitly find these views from the layout before you start using it.

The name of these attributes is from the corresponding view of the id, so we take id when the time to be very careful, because they will be a very important part of our class. The type of these attributes is also from the XML, so we do not need to carry out additional types of conversion.

One of the advantages of Kotlin Android `Extensions` is that it does not need to rely on other additional libraries in our code. It is only by the plug-in layer, when needed to generate the code needed to work, only need to rely on Kotlin's standard library.

How does it work behind it? The plugin will replace the function call function, such as access to the view and has a cache function, so that each time the property is called to get the view again. It should be noted that this cache will only be effective in `Activity` or `Fragment`. If it is added in an extension function, the cache will be skipped because it can be used in the `Activity` but the plugin can not be modified, so there is no need to add a cache function.
