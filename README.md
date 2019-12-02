# NodeJS-SQLServer-WindowsAuthentication

**Access SQL Server database using NodeJS/Express framework and mssql/msnodesqlv8 modules**

**dbconfig.js**

> The NodeJS app will listen on port 5000 and access database configuration stored under config/dbconfig.js

```javascript
// dbconfig.js
module.exports = {
  dbconfig: {
    server: "server_name",
    database: "db_name",
    options: {
      trustedConnection: true,
      encrypt: false
    }
  }
};
```

**app.js**

> The app has the needed modules including dbconfig.js, express, mssql and msnodesqlv8

```javascript
// app.js
const dbconfig = require("./config/db").dbconfig;
const express = require("express");
const sql = require("mssql/msnodesqlv8");
```

> Opening a port on Express

```javascript
// app.js
const app = express(); //Express App
const PORT = process.env.PORT || 5000; //Set port number
//open the port
app.listen(
  PORT,
  console.log(`Server started on port http://localhost:${PORT}`)
);
```

> Accessing the SQL Database using sql.connect

```javascript
// app.js
sql.connect(dbconfig, function(err) {
  if (err) {
    console.log(err);
  } else console.log(`Server connected to Database: ${dbconfig.database} \n`);
  //Request and queries goes here
});
```

> Submitting a request to db to retirve information

```javascript
// app.js
sql.connect(dbconfig, function(err) {
  //Check for any error, else connect
  if (err) {
    console.log(err);
  } else console.log(`Server connected to Database: ${dbconfig.database} \n`);

  // create Request object
  var request = new sql.Request();

  // query to the database and get the records
  request.query("select top 10 * from Demographics", function(err, recordset) {
    //print all values of a specific columns using foreach
    recordset["recordset"].forEach((element, index) => {
      console.log(`Value_${index}: ${element["Demographics_ID"]}`);
    });
  });
});
```

> Example Output on the Console log

```
Value_0: 122
Value_1: 124
Value_2: 145
Value_3: 148
Value_4: 235
Value_5: 250
Value_6: 345
Value_7: 556
Value_8: 595
Value_9: 656
```
