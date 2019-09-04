# CSS Notes #


- http://breakpoint-sass.com/
- https://github.com/at-import/Singularity


## Recommended Defaults ##

```css
* {
  margin: 0;
  padding: 0;
  word-wrap: break-word;
}
```


### Commonly Used Styles ###

```css
.simpleFormInput {
  border-radius: 3px;
  border: 1px solid transparent;
  border-top: none;
  border-bottom: 1px solid #DDD;
  outline: 0;
}

.formInputShadow {
  box-shadow: inset 0 1px 2px rgba(0,0,0,.39), 0 -1px 1px #FFF, 0 1px 0 #FFF;
}

.dropShadow {
  box-shadow: 0 3px 1px -2px rgba(0,0,0,.2), 0 2px 2px 0 rgba(0,0,0,.14), 0 1px 5px 0 rgba(0,0,0,.12);
  box-shadow: 0 1px 1px 0 rgba(0, 0, 0, 0.2), 0 3px 10px 0 rgba(0, 0, 0, 0.19);
  box-shadow: 0 10px 25px 0 rgba(0,0,0,.14);
}

.envelopeWindow {
  box-shadow: inset 0px 0px 50px -9px rgba(0,0,0,0.9);
  border-radius: 10px;
}

.subtleVignette {
  box-shadow: inset 0px 0px 150px 0px rgba(100,100,100,0.2);
}
```


```
.overlay-container {
  width: 100%; 
  position: relative; 
  overflow: scroll;
  box-shadow: inset 0px 0px 100px -10px rgba(0,0,0,0.75);
  border-radius: 5px;
}

.overlay {
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  position: absolute;
  overflow: scroll;
  background-color:rgba(255, 255, 255, 0.9);
  background: url(data:;base64,iVBORw0KGgoAAAANSUhEUgAAAAIAAAACCAYAAABytg0kAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAgY0hSTQAAeiYAAICEAAD6AAAAgOgAAHUwAADqYAAAOpgAABdwnLpRPAAAABl0RVh0U29mdHdhcmUAUGFpbnQuTkVUIHYzLjUuNUmK/OAAAAATSURBVBhXY2RgYNgHxGAAYuwDAA78AjwwRoQYAAAAAElFTkSuQmCC) repeat scroll transparent\9; /* ie fallback png background image */
}

```

  

## Mobile Friendly Content ##

```css
{
  overflow-y: scroll;
  -webkit-overflow-scrolling: touch; 
}
```


## Layouts ##


Vertically and Horizontally centered content:
```
.parent { display: table; }
.child { display: table-cell; vertical-align: middle; }
```


### Flexbox ###

http://www.flexboxpatterns.com/home


```css
.flexbox-container {
  margin: auto;
}

.defaults {
  flex: 0 1 auto;
  overflow: visible;
  min-height :auto;
}
```


Centered content
```css
{
  display: flex; 
  align-items: center; 
  justify-content: center; 
}
```


## Floats ##

```css
.group:after {
  content: "";
  display: table;
  clear: both;
}
```

```html
<div class="group">
  <div class="is-floated"></div>
  <div class="is-floated"></div>
  <div class="is-floated"></div>
</div>
```


## Troubleshooting ##

Scrolling/100% height issue - Safari Fix
```css
{
  position: absolute;
}
```

Some weird padding/margin issue, needing to scroll a little bit
```css
body {
  height: 100vh;
}
```

Anchors not taking up full space of li
```css
li {}
li a {
  padding: 20px;
  display: block;
}
```

Flexbox not using full height
```css
{
  min-height: -moz-min-content; // not implemented
  min-height: -webkit-min-content // works for opera and safari
  min-height: min-content // works for chrome
}
```

## Random ##

```
is-* state
has-* content

is-active
is-collapsed
is-hidden
is-visible

has-text
has-text-centered
has-text-aligned
```
