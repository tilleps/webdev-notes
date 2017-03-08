# Postgres #


## Resources ##

- https://help.ubuntu.com/community/PostgreSQL
- https://www.postgresql.org/download/linux/ubuntu/
- http://tecadmin.net/postgresql-cheat-sheet/
- http://shisaa.jp/postset/postgresql-full-text-search-part-1.html
- https://www.compose.com/articles/postgraphql-postgresql-meets-graphql/


## Install on Ubuntu 16.04 ##

```
echo "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main" | tee /etc/apt/sources.list.d/pgdg.list


wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | \
  sudo apt-key add -
sudo apt-get update

apt-get install postgresql-9.5
```


sudo -u postgres psql postgres
\password postgres


## DBeaver ##

Error while installing:

xattr -rc DBeaver.app


## Gotchas ##

- If you use table names that have mixed/upper casing, then you must double quote '"tbl1"'


## Troubleshooting ##


*error: role "postgres" does not exist*

```
createuser -s postgres
```



## Sysadmin ##

### OSX Binary Locations ###
```
/Applications/Postgres.app/Contents/Versions/9.4/bin/psql
```


### SSH Tunnel ###
```
ssh -nNT -L 5432:localhost:5432
```


### Backup ALL ###
```
sudo -i -u postgres pg_dumpall -U postgres > backups/postgres.sql
sudo -i -u postgres pg_dumpall -U postgres | gzip > backups/postgres.sql.gz
```

### Restore ALL ###
```
psql -f infile postgres
gunzip -c backups/postgres.sql.gz | psql -U postgres
```

### Backup ###
```
sudo -i -u postgres pg_dump db1 > backups/db.db1.sql
sudo -i -u postgres pg_dump db1 | gzip > backups/db.db1.sql.gz
```

### Restore ###
```
sudo -i -u postgres psql db1 < backups/db.db1.sql
gunzip -c backups/db.db1.sql | psql -U postgres
```


Check Timezone
```
SELECT  current_setting('TIMEZONE');
```



## Commands ##


### Login ###

```
su - postgres
psql
```
or
```
sudo -i -u postgres psql
```


```
\g    Equivalent to MySQL's \G
\x    "Expanded mode" Vertical formatting - Equivalent to MySQL's \G
```

### Database ###

```
\l          SHOW DATABASES (list databases)
\l+         More info
\c db_name  USE database (connect to database)

CREATE DATABASE db1
DROP DATABASE db1
```


### Table ###

```
\dt             SHOW TABLES (describe tables)
\d "tbl_name"   DESCRIBE TABLE tbl_name
\d+             More info

CREATE TABLE "tbl1" (
  id SERIAL PRIMARY KEY,
  id INT4 DEFAULT nextval('foo_seq') NOT NULL,
  name VARCHAR(100),
  description TEXT,
  date DATE
  timestamp TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
  CHECK(EXTRACT(TIMEZONE FROM my_timestamp) = '0'),
  tsv TSVECTOR
)


ALTER TABLE tbl1 ADD col_name VARCHAR(200)
ALTER TABLE tbl1 DROP col_name


ALTER TABLE tbl1 ADD COLUMN tsv TSVECTOR
CREATE INDEX tsv_idx ON tbl1 USING GIN(tsv)

CREATE UNIQUE INDEX user_identity_idx ON "User"((identities->>'user_id'), (identities->>'provider'), (identities->>'connection'));


ALTER TABLE "tbl1" RENAME TO "tbl2"


TRUNCATE tbl1 RESTART IDENTITY CASCADE

```


Generate Create Table Queries
```
sudo -i -u postgres pg_dump postgres --schema-only -t '"tbl1"'
```



```
SMALLSERIAL       [SERIAL2]
SERIAL            [SERIAL4] is 32-bit
BIGSERIAL         [SERIAL8] is 64-bit

UUID

SMALLINT          [INT2]
INT               [INT4]
BIGINT            [INT8]
REAL              [FLOAT4]
DOUBLE PRECISION  [FLOAT8]

CIDR        
INET              IP Address

DATE              Y-m-d
TIME
TIMETZ
TIMESTAMP
TIMESTAMPTZ

CHAR(n)
VARCHAR(n)

MONEY
NUMERIC
DECIMAL(10,2)

BOOLEAN     

BYTEA             Byte array (Binary data)
TEXT


```


### Roles/Users ###

`USER = ROLE + LOGIN`

```
\du       list users

GRANT ALL PRIVILEGES ON DATABASE db1 TO role1
ALTER ROLE role1 CREATEROLE CREATEDB SUPERUSER

CREATE ROLE role1 LOGIN;
CREATE USER role1 WITH PASSWORD 'pass123';
CREATE ROLE role1 WITH LOGIN PASSWORD 'jw8s0F4' VALID UNTIL '2005-01-01';
CREATE ROLE role1 WITH CREATEDB CREATEROLE;

DROP ROLE role1
```





## Queries ##

```
SELECT currval(pg_get_serial_sequence('Users', 'id'))
INSERT INTO users (name, age) VALUES ('Bob', 10) RETURN id
```

```
  # List Tables
  SELECT table_name FROM information_schema.tables WHERE table_schema='public';
```

```
  # List Databases:
  SELECT datname FROM pg_database WHERE datistemplate = false;
```

Timezone related
```
SHOW timezone;
SET TIME ZONE 'UTC';
SELECT NOW()
```


### Sequences ###



SELECT sequence_name FROM information_schema.sequences WHERE sequence_schema='public';
DROP SEQUENCE IF EXISTS sequence_name;

CREATE SEQUENCE foo_seq START 101;
SELECT currval('foo_seq');
SELECT nextval('foo_seq');
SELECT setval('foo_seq', 42);
SELECT setval('foo_seq', 42, true);

SELECT nextval('foo_seq', 42, false);  # nextval will return 42

INSERT INTO Users VALUES (nextval('foo_seq'), 'Bob');

BEGIN;
COPY Users FROM 'input_file';
SELECT setval('foo_seq', MAX(id)) FROM Users;
END;


### Full Text Search ###

- https://antjanus.com/blog/tutorials/using-postgresql-as-a-search-engine/



```
ALTER TABLE posts ADD COLUMN searchtext TSVECTOR;
UPDATE posts SET searchtext = to_tsvector('english', title || '' || content || '' || tags)
CREATE INDEX searchtext_gin ON posts USING GIN(searchtext);
CREATE TRIGGER ts_searchtext BEFORE INSERT OR UPDATE ON posts FOR EACH ROW EXECUTE PROCEDURE tsvector_update_trigger('searchtext', 'pg_catalog.english', 'title', 'content', 'tags')
SELECT * FROM posts where searchtext @@ to_tsquery('keyword');
```


