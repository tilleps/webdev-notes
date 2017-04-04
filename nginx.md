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