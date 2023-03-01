# Browser Artefacts Windows

### Key File Paths

Various browser artefacts can be found in a Windows users home directory under the following path:
```C:\Users\<USERNAME>\AppData\Local\<VENDOR>\<BROWSER>\User Data\Default\```

For example, the paths for edge, chrome, and firefox are as follows:
Chrome - ```C:\Users\<USERNAME>\AppData\Local\Google\Chrome\User Data\Default\History```
Edge - ```C:\Users\<USERNAME>\AppData\Local\Microsoft\Edge\User Data\Default\History```
Firefox - ```C:\Users\<USERNAME>\AppData\Roaming\Mozilla\Firefox\Profiles\*.default-release\places.sqlite```

### Viewing

There are a number of ways to view these history files, one way is to open up the database files in sqlite and browse through as you would with any database.

Another way, would be to use **Nirsoft package** which has built-in tools for analysing browser artefacts.

### SQLITE

DB Browser for SQLite can be used to find relevant browser artefacts. It allows for sql queries against a database to make it easier to search for what you are specifically looking for. Some example queries that would be useful when dealing with firefox history are as follows:
Retrieve all columns and rows from the moz_places table - ```SELECT * from moz_places```
Select only url and title from moz_places - ```SELECT url, title from moz_places```
Select only rows from url which contain github - ```SELECT url, title from moz_places WHERE url like "%github%"```

### Index.dat

The index.dat file is a database of recent browser activity for internet explorer. **Index.dat Analyser** can be used to locate and access the information in this database. [Index.dat Analyser link](https://www.systenance.com/download.php)