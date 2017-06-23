# Execute requests outside the main thread

As you know, HTTP requests are not allowed to execute in the main thread, otherwise it throws an exception. This is because blocking the UI thread is a very poor experience. The common practice in Android is to use `AsyncTask`, but these classes are very ugly and it is very difficult to use them without any side effects. If you use carelessly, `AsyncTasks` will be very dangerous, because when the runtime runs to `postExecute`, if the Activity has been destroyed, it will crash.

Anko provides a very simple DSL to handle asynchronous tasks, which meet most of the requirements. It provides a basic `async` function for executing code in other threads, or you can choose to return to the main thread by calling `uiThread`. Executing a request in a child thread is as simple as this:

```kotlin
async() {
    Request(url).run()
    uiThread { longToast("Request performed") }
}
```

`UIThread` has a very nice point that can be dependent on the caller. If it is called by a `Activity`, then`activity.isFinishing()` returns `true`, then `uiThread` will not execute, so that it will not crash when the activity is destroyed.

If you want to use `Future` to work,` async` returns a Java `Future`. And if you need a return to the results of `Future`, you can use `asyncResult`.

Really simple, right? And is more readable than `AsyncTasks`. Now, I just sent a url to the request to test whether we can correctly receive the content, so that we can draw it in the Activity. I will soon be talking about how to go to json parsing and converting the data classes into app, but it is also important to learn what data classes are before we continue.

Check the code and review the url request and the package structure of the code. You can run the app and make sure you can print on the json log and after the request is finished toast.
