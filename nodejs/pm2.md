# PM2 Notes #


```
PM2_HOME=/var/opt/pm2
```


```bash
pm2 logs $name --raw
pm2 logs $name --raw | bunyan


pm2 --update-env restart $name

ssh $hostname 'bash -l -c "sudo pm2 --update-env restart $pm2name"'
```



