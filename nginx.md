# Nginx Notes #



## Usage ##


**Start**
```
nginx
```

**Reload**
```
nginx -s reload
```



## Common Configs ###


### Service Check ###


```
location /__STATUS__/node {
  add_header Content-Type text/plain;
  return 200 'up';
}
```



## Dynamic Configs ##

server {
    server_name ~^(?<app_env>.*)\.domain\.com;
    
    # Dynamic Route
    location / {
      default_type text/html;
      return 200 $app_env;
    }
}




/etc/init/nginx.conf
```
  # nginx

  description "nginx http daemon"
  author "Eugene Song <tilleps@gmail.com>"

  start on (filesystem and net-device-up IFACE=lo)
  stop on runlevel [!2345]

  env DAEMON=/usr/sbin/nginx
  env PID=/var/run/nginx.pid

  expect fork
  respawn
  respawn limit 10 5
  #oom never

  pre-start script
          $DAEMON -t
          if [ $? -ne 0 ]
                  then exit $?
          fi
  end script

  exec $DAEMON
```