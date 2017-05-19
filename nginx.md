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
