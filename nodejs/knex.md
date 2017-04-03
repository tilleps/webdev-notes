# Knex #



```
  var fs = require('fs');

  exports.up = function(knex, Promise) {
    var sql = fs.readFileSync(__dirname + '/sql/123456789_setup.up.sql').toString();
    return knex.raw(sql);
  };

  exports.down = function(knex, Promise) {
    var sql = fs.readFileSync(__dirname + '/sql/123456789_setup.down.sql').toString();
    return knex.raw(sql);
  };
```


```
  mkdir -p config/dbs/mysql
  cd !$
  knex init  
```



```
var fs = require('fs');

exports.seed = function(knex, Promise) {
  var sql = fs.readFileSync(__dirname + '/sql/seed_name.sql').toString();
  return knex.raw(sql);
};
```



```
knex --knexfile ./config/dbs/postgres/knexfile.js migrate:make setup
knex --knexfile ./config/dbs/postgres/knexfile.js migrate:make step1  

knex --knexfile ./config/dbs/postgres/knexfile.js migrate:latest
knex --knexfile ./config/dbs/postgres/knexfile.js migrate:latest --env production 

knex --knexfile ./config/dbs/postgres/knexfile.js migrate:currentVersion
knex --knexfile ./config/dbs/postgres/knexfile.js migrate:rollback

knex --knexfile ./config/dbs/postgres/knexfile.js seed:make seed_name
knex --knexfile ./config/dbs/postgres/knexfile.js seed:run
```
