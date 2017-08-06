# What do we get through Kotlin?

Without diving too deep in Kotlin language (we’ll learn everything about it throughout this book),
these are some interesting features we miss in Java:

## Expresiveness

With Kotlin, it’s much easier to avoid boilerplate because most common situations are covered by
default in the language. For instance, in Java, if we want to create a data class, we’ll need to write
(or at least generate) this code:
```java
public class Artist {
    private long id;
    private String name;
    private String url;
    private String mbid;

    public long getId() {
        return id;
    }


    public void setId(long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getMbid() {
        return mbid;
    }

    public void setMbid(String mbid) {
        this.mbid = mbid;
    }

    @Override public String toString() {
        return "Artist{" +
          "id=" + id +
          ", name='" + name + '\'' +
          ", url='" + url + '\'' +
          ", mbid='" + mbid + '\'' +
          '}';
    }
}
```

With Kotlin, you just need to make use of a data class:

```kotlin
data class Artist(
    var id: Long,
    var name: String,
    var url: String,
    var mbid: String)
```

This data class auto-generates all the fields and property accessors, as well as some useful methods
such as `toString ()`

## Null Safety

When we develop using Java, a big part of our code is defensive. We need to check continuously
whether something is null before using it if we don’t want to find unexpected NullPointerException.
Kotlin, as many other modern languages, is null safe because we need to explicitly specify if an
object can be null by using the safe call operator (written `?`).

We can do things like this:
```Kotlin
// can not be compiled here. Artist can not be null
Var notNullArtist: Artist = null

// Artist can be null
Var artist: Artist ?= Null

// can not compile, artist may be null, we need to deal with
Artist.print ()

// print as soon as artist != Null
Artist?.print ()

// Smart conversion. If we had an empty check before, you would not need to use the secure call operator call
If (artist != Null) {
   Artist.print ()
}

// it is only possible to make sure that the artist is not null, otherwise it will throw an exception
Artist!!.Print ()

// use the Elvis operator to give a substitute value in the case of null
Val name = artist?.name?: "null"
```

## Extension functions

We can add new functions to any class. It’s a much more readable substitute to the usual utility
classes we all have in our projects. We could, for instance, add a new method to fragments to show
a toast:
```Kotlin
Fun Fragment.toast (message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText (getActivity (), message, duration) .show ()
}
```
We can do it now:
```Kotlin
Fragment.toast ("Hello world!")
```

## Functional support (Lambdas)

What if, instead of having to implement an anonymous class every time we need to implement
a click listener, we could just define what we want to do? We can indeed. This (and many more
interesting things) is what we get thanks to lambdas:
```Kotlin
View.setOnClickListener{toast ("Hello world!")}
```
This is only a small selection of what Kotlin can do to simplify your code. Now that you know some
of the many interesting features of the language, you may decide this is not for you. If you continue,
we’ll start with the practice right away in the next chapter.
