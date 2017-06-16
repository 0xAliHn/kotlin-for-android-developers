# What do we get through Kotlin?

Not in-depth Kotlin language (we will learn in the next chapter), there are some interesting features in Java:

## easy to show

With Kotlin, it is easier to avoid the template code because most of the typical cases are implemented in the default override in the language. For example, in Java, if we want a typical data class, we need to write (at least generate) the code:
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

Using Kotlin, we only need to pass the data class:
```kotlin
data class Artist(
    var id: Long,
    var name: String,
    var url: String,
    var mbid: String)
```

This data class, it will automatically generate all the attributes and their accessors, as well as some useful methods, such as `toString ()`

## SAFE null

When we use Java development, our code is mostly defensive. If we do not want to encounter `NullPointerException`, we need to keep using it to determine if it is null. Kotlin, as many modern languages, null safe, because we need to call the operator via a secure (written as `?`) To explicitly specify whether an object can be null

We can write like this:
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

## extended method

We can add functions to any class. It is more readable than the typical tool classes in our project. For example, we can add a copy of the show toast function:
```Kotlin
Fun Fragment.toast (message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText (getActivity (), message, duration) .show ()
}
```
We can do it now:
```Kotlin
Fragment.toast ("Hello world!")
```

## Function support (Lambdas)

Every time we declare a click on the triggered event, you can just define what we need to do, rather than having to implement an inner class? We really can do that, this (or other more of the events we are interested in) we need to thank lambda:
```Kotlin
View.setOnClickListener{toast ("Hello world!")}
```
Here's just picking up a small part of Kotlin that can simplify our code. Now that you already know some of the interesting features of this language, you can consider whether it is right for you. If you choose to continue, we will start our practice trip in the next chapter.
