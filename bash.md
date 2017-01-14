# Bash Notes #


http://www.davidpashley.com/articles/writing-robust-shell-scripts/



Check Operating System and Version
```
lsb_release -a
```



```
sudo usermod -a -G groupName userName
```


Convert to mp3
```
ffmpeg -i "file.m4a" -acodec libmp3lame -ab 128k "file.mp3" 
```

## Postgres ##


SSH Tunnel

```
ssh -nNT -L 5432:localhost:5432
```

Backup Postgres

```
sudo -i -u postgres pg_dumpall -U postgres > backups/postgres.sql
sudo -i -u postgres pg_dumpall -U postgres | gzip > backups/postgres.sql.gz
```

Restore Postgres
```
psql -f infile postgres
gunzip -c backups/postgres.sql.gz | psql -U postgres
```


## IPTables ##


List Rules
```
iptables -t nat -L
```

Add Rule
```
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 10022 -j DNAT --to 10.199.199.144:22
```

Delete Rule
```
iptables -t nat -D PREROUTING -i eth0 -p tcp --dport 10022 -j DNAT --to 10.199.199.144:22
```
