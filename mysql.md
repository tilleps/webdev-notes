# MySQL



## View Encoding


```sql
USE db_name;
SELECT @@character_set_database;
```


```sql
ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;
ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```



## Add Index Without Locking Table

```sql
ALTER TABLE session ADD INDEX expires_idx (expires ), ALGORITHM=inplace, LOCK=NONE;
```