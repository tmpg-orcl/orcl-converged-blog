## orcl-converged-blog
```
SINGLE-PURPOSE DATABASE
```

```
1 # INSTALL NPM MODULES
2 npm i mongo
3 npm i neo4j-driver
4 npm i mysql
```

```js
1   // JS
2   // ...
3   // CONNECT TO MONGODB CLIENT
4   let mongoClient = require('mongodb').MongoClient; // includes Mongodb module
5      MongoCLient.connect(mongoURL,mongoOptions, (err, client) => {
6          if (err) {/* ERROR HANDLING */} else {/* CONNECTED TO MONGODB */}
7      });
8   // CONECT TO NEO4J
9   const neo4j = require('neo4k-driver'); //include Neo4j module
10  const driver = neo4j.driver(uri, neo4j.auth.basic(user, password));
11  const neo4jSession = driver.session();
12      try {
13        const result = await neo4jSession.run(/* CONNECTED TO NEO4J */);
14      } finally {
15        await session.close();
16      }
17  // CONECT TO MYSQL CLIENT
18  var mysql = require('mysql'); 
19  var mysqlConnection = mysql.createConnection({/* CONNECTION INFO */})
20  mysqlConnection.connect((err) => {
21      if(err) {/* ERROR HANDLING */} else {/* CONNECTED TO MYSQL */}       
22  });
23  // ...
```

```
ORACLE CONVERGED DATABASE
```

```
1 # INSTALL NPM MODULES
2 npm i oracledb
3
4
```

```js
1   // JS
2   // ...
3   // CONNECT TO ORACLE DB CLIENT
4   const oracledb = require('oracledb');
5   function otacleDbConnect(dbOptions) {
6       return new Promise(async (resolve, reject) => {
7           try {
8               await oracledb.getConnection(dbOptions).then(conn => {
9                   resolve(/* CONNECTED TO ORACLE DB */);
10              });
11          } catch (err) {     
12              reject(err);  
13          } 
14      });
15  }
16
17
18 
19
20
21       
22 
23  // ...
```

```
SINGLE-PURPOSE DATABASE
```

```js
1   // JS
2   // The Objective of the query is to return the user's session history of any BnB booked on 12/20/2020 which included 'friends' the user had been connected with for over 3 years
3   // The variables "user_id", "bookings_data", "friendship_length" predefined
4
5   // Step 1.a: Lets apporach this problem by querying and pulling all the "bookingIds" the user had on 12/20/2020 from the MySql Database
6    // ...
7   mysqlBookingIds = [];
8   mysqlConnection.query('SELECT BOOKING_ID FROM USER_BOOKINGS WHERE USER_ID = ? AND BOOKING_DATE = ? AS bookings', [user_id, booking_date], (error, results, fields) => {
9       if (error) throw error;
10      // Step 1.b: Clean 'mysqlBookingIds' for use in next step (think ETL)
11      mysqlBookingIds = transformResult(results) {/* ... */};
12  });     
13
14  mysqlConnection.end();
15  
16  // Step 2.a: Then, using the bookingIds, determine which bookings had been with 'friends' the user had been connected with for over 3 years by returning results from Neo4j
17  neo4jFilteredBookingIds = [];
18  session.run(
19      'MATCH (u:Person)-[:IS_FRIENDS_WITH]-(friends) WHERE u.id = $user_id AND friends.friendship_length > $fLength RETURN FILTER(x in friends.booking_ids WHERE x IN $userBookingIds)',
20      { userId: user_id,
21        fLength: friendship_length,
22        userBookingIds: mysqlBookingIds
23      }).then(result => {
24          // Step 2.b: Clean 'nep4jFilteredBookingIds' for use in next step (think ETL)
25          neo4jFilteredBookingIds = transformResult(result.records) {/* ... */};
26      }).catch (error => {
27          console.log(error);
28      }).then(() => session.close())
29  // Step 3: Finally, using nep4jFilteredBookingIds, return the user's session history from MongoDB
30  finalResult = [];   
31  MongoClient.connect(url,monoOptions, (err, client) => {
32      if(err) console.log(err);
33      const db = client.db(DB_NAME);
34      const userHistoryCollection = db.collection(COLLECTION_USER_HISTORY);
35      userHistoryCollection.find({"user_id":{$eq: [user_id]}}, {"session_history_of_booking": {"$in": [neo4jFilteredBookingIds]}}).toArray((err, items) => {
36          if (err) console.log(err);
37          finalResult = items;
38      })
39  });
40
41  console.log(finalResults); // Print final result
42  // ...
```

```
ORACLE CONVERGED DATABASE
```

```js
1   // JS
2   // The Objective of the query is to return the user's session history of any BnB booked on 12/20/2020 which included 'friends' the user had been connected with for over 3 years
3   // The variables "user_id", "bookings_data", "friendship_length" predefined
4
5   // Step 1: All of the data resides within a single-pluggable database, split between three different tables. The data can be joined together and pulled out with a single PL/SQL statement.
6   // ...
7   finalResult = [];
8   oracledb.execute('', [], {outFormat: oracledb.ARRAY}
9       ).then(result => {
10          finalResult = result;
11      }).catch(err => {
12          console.log(err);
13      });
14
15  console.log(finalResult); // Print final result
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42 // ...
```