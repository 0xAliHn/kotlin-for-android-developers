# Create a SQLiteOpenHelper

As you know, Android uses SQLite as its database management system. SQLite is a database embedded in the app, it is really very light. That's why this is a good choice for mobile app app.

Nevertheless, its API for operating the database is very native in Android. You will need to write a lot of SQL statements and your object with `ContentValues` or `Cursors` between the parsing process. Very grateful to the joint use of Kotlin and Anko, we can greatly simplify these.

Of course, there are a lot of Android can be used on the database library, thanks to Kotlin interoperability, all of these libraries can be used normally. But for a simple database can not use any of them, after a minute you can see.

