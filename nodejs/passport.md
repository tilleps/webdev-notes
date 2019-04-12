# PassportJS


```js
//  Example passport example
router.post('/login', 
  bodyParser.urlencoded({ limit: '50mb', extended: false }),
  function (req, res, next) {
    passport.authenticate('local', { 
      successReturnToOrRedirect: '/', 
      failureRedirect: '/login' 
    })(req, res, next);  
  }
);


//
//  Custom Authentication Handling
//
router.post('/login', 
  bodyParser.urlencoded({ limit: '50mb', extended: false }),
  bodyParser.json({ limit: '50mb' }),
  function (req, res, next) {
  
    passport.authenticate('local', function (err, user, info) { 
    
      console.log('user', user);
      console.log('info', info);
    
      if (err) {
        return next(err); // will generate a 500 error
      }
      // Generate a JSON response reflecting authentication status
      if (!user) {
        res.status(401);
        return res.send({ success : false, message : 'authentication failed' });
      }
      req.login(user, function(err){
        if(err){
          return next(err);
        }
        return res.send({ 
          success : true, 
          message : 'authentication succeeded', 
          payload: { 
            user: user 
          } 
        });        
      });
      
    })(req, res, next);
  
  }
);
```
