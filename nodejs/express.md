# ExpressJS Notes #


## Requests/Responses ##


Determine if to send JSON format
```
req.accepts('html, json') === 'json'
```


jQuery $.ajax setting:
```
$.ajax({
  dataType: "json",
});
```




## Sessions ##

```javascript
app.use(session({
  saveUninitialized : true,
  resave            : true,
  secret            : config.session.secret,
  store             : session.MemoryStore(),
  key               : 'authclient.sid', // This should be unique to other apps while developing
  cookie            : {
    secure: false,
    maxAge: config.session.maxAge
  },
}));
```

