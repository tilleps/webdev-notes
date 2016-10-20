# Javascript Notes #


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

