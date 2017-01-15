# Bash Notes #


http://www.davidpashley.com/articles/writing-robust-shell-scripts/


export HISTCONTROL=ignoreboth:erasedups
Add to .bashrc to avoid duplicate entries


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


## Shortcuts ##

```
  Ctrl+R      search history
              - then Ctrl-r again to show next match
              - then Tab to show all options

  Ctrl+A      move to beginning of line
  Ctrl+E      move to end of line
  Ctrl+K      cut from position to end of line
  Ctrl+U      cut from position beginning of line

  Esc-D       cut/delete by a word (forward)
  Ctrl+W      cut/delete by a word (backward)
  Ctrl+Y      paste
  
  Esc-F       move forward by a word
  Esc-B       move backward by a word
  Ctrl+F      move foward a character
  Ctrl+B      move backward a character
```
  
```
  cd -        change to last dir
  pushd <dir> mark current dir and go to <dir>
  popd        go to marked dir 

  less        better than cat, doesn't flood screen, same keys
  find        find files, e.g. find / -name <filename>
  !!          last command, e.g. sudo !!
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
