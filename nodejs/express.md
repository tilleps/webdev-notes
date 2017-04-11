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


```javascript
//
//  Session (Cookie)
//  Note: Requires cookie-parser
//

var cookieParser = require('cookie-parser');
var cookieSession = require('cookie-session');

//  Cookie Parser
app.use(cookieParser());

app.set('trust proxy', true);

app.use(cookieSession({
  //  Secure cookies behind proxies
  //  https://expressjs.com/en/guide/behind-proxies.html
  //  Need to set 'trust proxy' to true
  secure: true,
  name: config.session.key || 'authorization.sid',
  //keys: ['key1', 'key2'],
  secret: config.secrets.session,
  sameSite: true,
  //domain: '',
  maxAge: config.session.maxAge
}));
```
