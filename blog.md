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
// DEMONSTRATION PURPOSES ONLY
// JS
// ...
// CONNECT TO MONGODB CLIENT
let mongoClient = require('mongodb').MongoClient; // includes Mongodb module
    MongoCLient.connect(mongoURL,mongoOptions, (err, client) => {
       if (err) {/* ERROR HANDLING */} else {/* CONNECTED TO MONGODB */}
   });
// CONECT TO NEO4J
const neo4j = require('neo4k-driver'); //include Neo4j module
const driver = neo4j.driver(uri, neo4j.auth.basic(user, password));
const neo4jSession = driver.session();
    try {
      const result = await neo4jSession.run(/* CONNECTED TO NEO4J */);
    } finally {
      await session.close();
    }
// CONECT TO MYSQL CLIENT
var mysql = require('mysql'); 
var mysqlConnection = mysql.createConnection({/* CONNECTION INFO */})
mysqlConnection.connect((err) => {
    if(err) {/* ERROR HANDLING */} else {/* CONNECTED TO MYSQL */}       
});
// ...
// DEMONSTRATION PURPOSES ONLY
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
// DEMONSTRATION PURPOSES ONLY
// JS
// ...
// CONNECT TO ORACLE DB CLIENT
const oracledb = require('oracledb');
function otacleDbConnect(dbOptions) {
    return new Promise(async (resolve, reject) => {
        try {
            await oracledb.getConnection(dbOptions).then(conn => {
                resolve(/* CONNECTED TO ORACLE DB */);
            });
        } catch (err) {     
            reject(err);  
        } 
    });
}
// ...
// DEMONSTRATION PURPOSES ONLY
```

```
SINGLE-PURPOSE DATABASE
```

```js
// DEMONSTRATION PURPOSES ONLY
// JS
// Use Case: Host-a-BnB (vacation rental company) wants to build a dashboard that visualizes sessions data of users who booked BnBs at a given date with long term friends (> 5 year). 
// The query below will pull booking sessions data using predefined variables VAR_USER_ID, VAR_BOOKING_DATE, and VAR_FRIENDSHIP_LENGTH.
// Step 1.a: Lets apporach this problem by querying and pulling all the "bookingIds" the user had on the BOOKING_DATE from the MySql Database
// ...
mysqlBookingIds = [];
mysqlConnection.query('SELECT BOOKING_ID FROM BOOKINGS WHERE USER_ID = ? AND BOOKING_DATE = ? AS bookings', [VAR_USER_ID, VAR_BOOKING_DATE], (error, results, fields) => {
    if (error) throw error;
    // Step 1.b: Clean 'mysqlBookingIds' for use in next step (think ETL)
    mysqlBookingIds = transformResult(results) {/* ... */};
});     
mysqlConnection.end();
// Step 2.a: Then, using the bookingIds, query which bookings had friends with VAR_FRIENDSHIP_LENGTH > 5 years from Neo4j
neo4jFilteredBookingIds = [];
session.run(
    'MATCH (u:Person)-[:IS_FRIENDS_WITH]-(friends) WHERE u.id = $user_id AND friends.friendship_length > $fLength RETURN FILTER(x in friends.booking_ids WHERE x IN $userBookingIds)',
    { userId: VAR_USER_ID,
      fLength: VAR_FRIENDSHIP_LENGTH,
      userBookingIds: mysqlBookingIds
    }).then(result => {
        // Step 2.b: Clean 'nep4jFilteredBookingIds' for use in next step (think ETL)
        neo4jFilteredBookingIds = transformResult(result.records) {/* ... */};
    }).catch (error => {
        console.log(error);
    }).then(() => session.close())
// Step 3: Finally, using nep4jFilteredBookingIds, return the user's booking session history from MongoDB
finalResult = [];   
MongoClient.connect(url,monoOptions, (err, client) => {
    if(err) console.log(err);
    const db = client.db(DB_NAME);
    const userHistoryCollection = db.collection(COLLECTION_USER_HISTORY);
    userHistoryCollection.find({"user_id":{$eq: [VAR_USER_ID]}}, {"session_history_of_booking": {"$in": [neo4jFilteredBookingIds]}}).toArray((err, items) => {
        if (err) console.log(err);
        finalResult = items;
    })
});
console.log(finalResults); // Print booking sessions history
// ...
// DEMONSTRATION PURPOSES ONLY
```

```
ORACLE CONVERGED DATABASE
```

```js
// DEMONSTRATION PURPOSES ONLY
// JS
// Use Case: Host-a-BnB (vacation rental company) wants to build a dashboard that visualizes sessions data of users who booked BnBs at a given date with long term friends (> 5 year). 
// The query below will pull booking sessions data using predefined variables VAR_USER_ID, VAR_BOOKING_DATE, and VAR_FRIENDSHIP_LENGTH.
// Step 1: All of the data resides within a single-pluggable database, split between three different tables. The data can be joined together and pulled out with a single PL/SQL statement.
// ...
finalResult = [];
oracledb.execute(
    `with booking_cte as ( 
    SELECT BOOKING_ID from BOOKINGS WHERE USER_ID = :1 AND BOOKING_DATE = to_date(:2, 'MM/DD/YY')
), relationship_cte as (
    SELECT USER_ID, FRIEND_ID, RELATIONSHIP_TYPE, FRIENDSHIP_LENGTH, BOOKING_ID FROM RELATIONSHIPS
    START WITH USER_ID = :3
    CONNECT BY NOCYCLE PRIOR
        USER_ID = FRIEND_ID
), session_history_cte as (
SELECT jt.BOOKING_ID, jt.USER_ID, jt.SESSION_HISTORY_DATA from SESSION_HISTORY,
    json_table( json_doc, '$'
        columns (
            user_id    number path '$.user_id',
            nested path '$.session_history_of_booking[*]' columns (
                booking_id number path '$.booking_id',
                session_history_data clob path '$.session_history_data'
        ))
    ) jt
)SELECT s.SESSION_HISTORY_DATA FROM booking_cte b 
    JOIN relationship_cte r ON r.BOOKING_ID = b.BOOKING_ID
    JOIN session_history_cte s ON s.BOOKING_ID = r.BOOKING_ID
WHERE r.FRIENDSHIP_LENGTH > :4`,
    [VAR_USER_ID, VAR_BOOKING_DATE, VAR_USER_ID, VAR_FRIENDSHIP_LENGTH], {outFormat: oracledb.ARRAY}
    ).then(result => {
        finalResult = result;
    }).catch(err => {
        console.log(err);
   });
console.log(finalResult); // Print booking sessions history
// ...
// DEMONSTRATION PURPOSES ONLY
```



Use Case: Host-a-BnB (vacation rental company) wants to build a dashboard that visualizes sessions data of users who booked BnBs at a given date with long term friends (> 5 year). 
The query below will pull booking sessions data using predefined variables "user_id", "bookings_data", and "friendship_length".

