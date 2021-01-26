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
4   // Assume data comes out in the  type we desire, no messing with conversion
5
6   // Step 1: Lets apporach this problem by pulling all the "bookingIds" the user had on 12/20/2020 from MySql
    // ...
7
8
9
10
11
12
13
14
15
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
42
43
44
45
46
```

```
ORACLE CONVERGED DATABASE
```

```js
1   // JS
2
3
4
5
6
7
8
9
10
11
12
13
14
15
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
42
43
44
45
46
```