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

```
server {
    server_name ~^(?<app_env>.*)\.domain\.com;
    
    # Dynamic Route
    location / {
      default_type text/html;
      return 200 $app_env;
    }
}
```


## Request ID ##

```
set $trace_id $connection-$connection_requests-$msec;
if ($http_x_request_id) {
  set $trace_id $http_x_request_id;
}

add_header X-Trace-ID $trace_id;
```