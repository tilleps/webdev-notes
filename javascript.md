# Javascript Notes #



## JSON ##


Keep undefined values in a JSON
```javascript
  //  Keep the undefined values
  function replacer(k, v) {
    return (typeof v === 'undefined') ? null : v;
  }
  
  console.log(JSON.stringify(module.exports, replacer, 2));
```


## FormData ##


```javascript
var form = document.getElementById('form');
var formData = new FormData(form);

var entry;
var values = {};
var entries = formData.entries();


//
//  ES5
//
while (!(entry = entries.next()).done) {
  values[entry.value[0]] = entry.value[1];
}


//
//  ES6
//
for(var entry of formData.entries()) {
   values[entry[0]] = entry[1];
}


```


## Async/Await ##

```
try {
  //  Serial loading
  await firstPromise();
  await secondPromise();
  
  //  Parallel loading
  await Promise.all([
    firstPromise(),
    secondPromise()
  ])
}
catch (e) {
  
}
```