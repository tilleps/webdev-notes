# Postgres #


https://help.ubuntu.com/community/PostgreSQL

https://www.postgresql.org/download/linux/ubuntu/


Install on Ubuntu 16.04
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


