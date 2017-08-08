# Extra function

Along with a data class, we get a handful of interesting functions for free, apart from the properties
we already talked about (which prevents us from writing getters and setters):

- equals(): it compares the properties from both objects to ensure they are identical.
- hashCode(): we get a hash code for free, also calculated from the values of the properties.
- copy(): you can copy an object, modifying the properties you need. We’ll see an example later.
- A set of numbered functions that are useful to map an object into variables. It will also be
  explained soon.
