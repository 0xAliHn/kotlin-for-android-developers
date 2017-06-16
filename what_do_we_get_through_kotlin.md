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
```kotlin
// 这里不能通过编译. Artist 不能是null
var notNullArtist: Artist = null

// Artist 可以是 null
var artist: Artist? = null

// 无法编译, artist可能是null，我们需要进行处理
artist.print()

// 只要在artist != null时才会打印
artist?.print()

// 智能转换. 如果我们在之前进行了空检查，则不需要使用安全调用操作符调用
if (artist != null) {
  artist.print()
}

// 只有在确保artist不是null的情况下才能这么调用，否则它会抛出异常
artist!!.print()

// 使用Elvis操作符来给定一个在是null的情况下的替代值
val name = artist?.name ?: "empty"
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
View.setOnClickListener {toast ("Hello world!")}
```
Here's just picking up a small part of Kotlin that can simplify our code. Now that you already know some of the interesting features of this language, you can consider whether it is right for you. If you choose to continue, we will start our practice trip in the next chapter.
