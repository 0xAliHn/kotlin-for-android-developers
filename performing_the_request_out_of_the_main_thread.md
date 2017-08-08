# Performing the request out of the main thread

As you may know, HTTP requests are not allowed to be done in the main thread, it will throw
an exception. This is because blocking the UI thread is a really bad practice. The common solution
in Android is to use an `AsyncTask`, But these classes are ugly and difficult to implement without
any side effects. `AsyncTasks` are dangerous if not used carefully, because by the time it reaches
`postExecute`, the activity could have been destroyed, and the task will crash.

Anko provides a very easy DSL to deal with asynchrony, which will fit most basic needs. It basically
provides an `async` function that will execute its code in another thread, with the option to return to
the main thread by calling  `uiThread`. Executing the request in a secondary thread is as easy as this:

```kotlin
async() {
    Request(url).run()
    uiThread { longToast("Request performed") }
}
```

A nice thing about `UIThread` is that it’s implemented differently depending on the caller object. If
it’s used by an `Activity`, the the `uiThread` code won’t be executed if `activity.isFinishing()` returns
`true`and it won’t crash if the activity is no longer valid.

You also can use your own executor:

```kotlin
val executor = Executors.newScheduledThreadPool(4)
async(executor) {
// Some task
}
```

`async` returns a java `Future` in case you want to work with futures. And if you need it to return a
future with a result, you can use `asyncResult`.

Really simple, right? And much more readable than `AsyncTasks`For now, I’m just sending a static
url to the request, to test that we receive the content properly and that we are able to draw it in
the activity. I will cover the json parsing and conversion to app data classes soon, but before we
continue, it’s important to learn what a data class is.

Check the code to review the url used by the request and some new package organisation. You can
run the app and check that you can see the json in the log and the toast when the request finishes