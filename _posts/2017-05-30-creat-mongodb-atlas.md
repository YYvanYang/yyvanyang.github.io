---
layout: post
title:  "Create a mongodb database on mongodb atlas"
categories: node
---

### Create a free account of mongodb

1, Go to the official site https://www.mongodb.com/, click start free button.
![](../images/start-free.png)

2, Fill in account profile and hit cotinue to submit sign up info.

3, After create successfully, then login with your new account.

### Create a new Cluster(database)
1, Build a new cluster first.

2, Get the connect string of the database.
  - click connect
    ![](../images/db-connect.png)
  - add IP whitelist
    You can add "0.0.0.0/0" by default. and then you can access it from anywhere.
    ![](../images/conn-string.png)
  - copy the URI Connection String
    Replace <PASSWORD> with the db user you created, and <DATABASE> with the name of the database to which you wish to connect.
    

### [Node.js Driver Example][1]

```javascript
var MongoClient = require('mongodb').MongoClient;

var uri = "mongodb://kay:myRealPassword@mycluster0-shard-00-00-wpeiv.mongodb.net:27017,mycluster0-shard-00-01-wpeiv.mongodb.net:27017,mycluster0-shard-00-02-wpeiv.mongodb.net:27017/admin?ssl=true&replicaSet=Mycluster0-shard-0&authSource=admin";
MongoClient.connect(uri, function(err, db) {
  db.close();
});
```

To connect to a database other than admin but still authenticate to the admin database, update the database component of the connection string.

```js
mongodb://username:password@host1:port1,...,hostN:portN/database?authSource=admin&...
```

For example, the following connection string specifies test in the database component and includes the authSource=admin option.

```js
// `kay` is the username
// `myRealPassword` is the password
var uriTestDb = "mongodb://kay:myRealPassword@mycluster0-shard-00-00-wpeiv.mongodb.net:27017,mycluster0-shard-00-01-wpeiv.mongodb.net:27017,mycluster0-shard-00-02-wpeiv.mongodb.net:27017/test?ssl=true&replicaSet=Mycluster0-shard-0&authSource=admin";
MongoClient.connect(uriTestDb, function(err, db) {
   db.close();
});
```

[1]: https://docs.atlas.mongodb.com/driver-connection/?_ga=2.20871103.645694338.1494771437-1700192153.1494719912#node-js-driver-example
