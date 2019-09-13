# Browsers



## Gotchas


```js
//  Prevent browsers from caching DOMContentLoaded event
//  Fix issue where back button disables submit button
window.onunload = function(){};
```