## Git Commands

- `git fetch –all`
- `git pull –all`
- `git branch –a`
- `git branch –D branchName`
- `git checkout –b branchName`
- `git checkout –b branchName origin/branchName`
- `gitk –all`


## Sequelize ORM


To use sequelize properly we must use sequelize table objects. To objects use we must define objects first.


### Quick Startup



```

  // Use `npm install sequelize` to add in project


  // Require sequelize module
  var Sequelize = require('sequelize');


  // Make database connection
  var dbConnect = new Sequelize('database', 'root', '', {
    host: "localhost",
    dialect: "mysql",
    port: 3306
  }),


  // Define database table object
  var tableObj = dbConnect.define('tableName', {

    id: {
      type: Sequelize.INTEGER,
      primaryKey: true,
      autoIncrement: true,
      allowNull: false
    },
    name: Sequelize.STRING,
    status: Sequelize.INTEGER
  } , {

    tableName : 'tableName'
  });


  // Use find method to fetch a record from database
  tableObj.find({
    where: {
      columnName: {}
    }
  }).then(function(result){
    console.log("all Data", result);
  });


```

###### By using find method we can have some options to use. [View Docs](http://docs.sequelizejs.com/en/latest/docs/models-usage/) eg:

```


  tableObj.find({
    where: {
      name: {
        $like: 'foo%'  // `name` LIKE 'foo%'
      }
    }
  }).then(function(result){
    console.log("all Data", result);
  });

  // SELECT * FROM tableName WHERE `name` LIKE 'foo%';

```


###### Some few more options to use.


- $gt: 6,                // id > 6
- $gte: 6,               // id >= 6
- $lt: 10,               // id < 10
- $lte: 10,              // id <= 10
- $ne: 20,               // id != 20
- $between: [6, 10],     // BETWEEN 6 AND 10
- $notBetween: [11, 15], // NOT BETWEEN 11 AND 15
- $in: [1, 2],           // IN [1, 2]
- $like: '%hat',         // LIKE '%hat'
- $notLike: '%hat'       // NOT LIKE '%hat'
- $iLike: '%hat'         // ILIKE '%hat' (case insensitive)
- $notILike: '%hat'      // NOT ILIKE '%hat'
- $overlap: [1, 2]       // && [1, 2] (PG array overlap operator)
- $contains: [1, 2]      // @> [1, 2] (PG array contains operator)
- $contained: [1, 2]     // <@ [1, 2] (PG array contained by operator)



###### We have some more methods to use. eg:


```

  // Find maximum value of id from table
  tableObj.max('id').then(function(result){
    console.log("all Data", result);
  });

  // SELECT MAX(`id`) FROM tableName;


  // Find maximum value of id with where clause from table
  tableObj.max('id', {
    where: {
      columnName: 'abcdef'
    }
  }).then(function(result){
    console.log("all Data", result);
  });

  // SELECT MAX(`id`) FROM tableName WHERE `columnName` = 'abcdef';

```


###### Some few more methods to use.


- `tableObj.update({ columnName1: 'columnValue', columnName2: 'columnValue' });`
- `tableObj.findOrCreate({ where: {columnName: 'columnValue'}, defaults: {columnName1: 'columnValue', columnName2: 'columnValue'}});`
- `tableObj.max('columnName');`
- `tableObj.min('columnName');`
- `tableObj.sum('columnName');`
- `tableObj.sum('columnName');`
- `tableObj.increment('columnName');`
- `tableObj.decrement('columnName');`



### Join with Sequelize ORM


Define objects of tables.


```


  // Define database first table object
  var User = sequelize.define('users', {

    id: {
      type: Sequelize.INTEGER,
      primaryKey: true,
      autoIncrement: true
    },
    name: Sequelize.STRING,
    status: Sequelize.INTEGER
  } , {

    tableName : 'users'
  });


  // Define database second table object
  var RoleUser = sequelize.define('role_user', {

    id: {
      type: Sequelize.INTEGER,
      primaryKey: true,
      autoIncrement: true
    },
    user_id: Sequelize.INTEGER,
    role_id: Sequelize.INTEGER,
    status: Sequelize.INTEGER
  } , {

    tableName : 'role_user'
  });


  // Define constraints of tables objects
  User.hasMany(RoleUser, {
    foreignKey: 'user_id'
  });

  RoleUser.belongsTo(User, {
    foreignKey: 'user_id'
  });


  // Use find method with join to fetch a record from database
  User.find({
    where: {
      columnName: {}
    },
    include: [RoleUser]
  });


```



## Validations


```


  // Define database table object
  var tableObj = dbConnect.define('tableName', {

    id: {
      type: Sequelize.INTEGER,
      primaryKey: true,
      autoIncrement: true
    },
    name: Sequelize.STRING,
    status: {
      type: Sequelize.INTEGER,
      validate: {
        isInt: true
      }
    }
  } , {

    tableName : 'tableName'
  });


  // Use find method to fetch a record from database
  tableObj.create({
    name: 'abcdef',
    status: 'active'
  }).then(function(result){
    console.log("all Data", result);
  }).catch(function(error){
    console.log("Validation Error: ", error.message);
  });


```


###### Some few more options to use for validation.


- is: ["^[a-z]+$",'i'],     // will only allow letters
- is: /^[a-z]+$/i,          // same as the previous example using real RegExp
- not: ["[a-z]",'i'],       // will not allow letters
- isEmail: true,            // checks for email format (foo@bar.com)
- isUrl: true,              // checks for url format (http://foo.com)
- isIP: true,               // checks for IPv4 (129.89.23.1) or IPv6 format
- isIPv4: true,             // checks for IPv4 (129.89.23.1)
- isIPv6: true,             // checks for IPv6 format
- isAlpha: true,            // will only allow letters
- isAlphanumeric: true      // will only allow alphanumeric characters, so "_abc" will fail
- isNumeric: true           // will only allow numbers
- isInt: true,              // checks for valid integers
- isFloat: true,            // checks for valid floating point numbers
- isDecimal: true,          // checks for any numbers
- isLowercase: true,        // checks for lowercase
- isUppercase: true,        // checks for uppercase
- notNull: true,            // won't allow null
- isNull: true,             // only allows null
- notEmpty: true,           // don't allow empty strings
- equals: 'specific value', // only allow a specific value
- contains: 'foo',          // force specific substrings
- notIn: [['foo', 'bar']],  // check the value is not one of these
- isIn: [['foo', 'bar']],   // check the value is one of these
- notContains: 'bar',       // don't allow specific substrings
- len: [2,10],              // only allow values with length between 2 and 10
- isUUID: 4,                // only allow uuids
- isDate: true,             // only allow date strings
- isAfter: "2011-11-05",    // only allow date strings after a specific date
- isBefore: "2011-11-05",   // only allow date strings before a specific date
- max: 23,                  // only allow values
- min: 23,                  // only allow values >= 23
- isArray: true,            // only allow arrays
- isCreditCard: true,       // check for valid credit card numbers



#### For custom validations.


```


  // Define database table object
  var tableObj = dbConnect.define('tableName', {

    id: {
      type: Sequelize.INTEGER,
      primaryKey: true,
      autoIncrement: true
    },
    name: Sequelize.STRING,
    status: {
      type: Sequelize.INTEGER,
      validate: {
        isEven: function(value) {
          if(parseInt(value) % 2 != 0)
            throw new Error('Only even values are allowed!')
        }
      }
    }
  } , {

    tableName : 'tableName'
  });


  // Use find method to fetch a record from database
  tableObj.create({
    name: 'abcdef',
    status: 'active'
  }).then(function(result){
    console.log("all Data", result);
  }).catch(function(error){
    console.log("Validation Error: ", error.message);
  });


```



#### For custom message.


```


  // Define database table object
  var tableObj = dbConnect.define('tableName', {

    id: {
      type: Sequelize.INTEGER,
      primaryKey: true,
      autoIncrement: true
    },
    name: Sequelize.STRING,
    status: {
      type: Sequelize.INTEGER,
      validate: {
        isInt: {
          msg: 'Status value must be integer.'
        }
      }
    }
  } , {

    tableName : 'tableName'
  });


  // Use find method to fetch a record from database
  tableObj.create({
    name: 'abcdef',
    status: 'active'
  }).then(function(result){
    console.log("all Data", result);
  }).catch(function(error){
    console.log("Validation Error: ", error.message);
  });


```