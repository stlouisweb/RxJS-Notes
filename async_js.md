# Netflix async JavaScript with Reactive Extensions

###Links
[https://www.youtube.com/watch?v=XRYN2xt11Ek&feature=youtu.be](Video)

[https://github.com/Reactive-Extensions/RxJS](Reactive Extensions JS)

[http://jhusain.github.io/learnrx/](Learn RX)


### Flexible functions

ForEach:

```javascript
[1, 2, 3].forEach(x => console.log(x))
```
logs 2, 3, 4 each on new line

Map:

```javascript
[1, 2, 3].map(x=> x +1) 
```

returns new collection [2, 3, 4]

Filter:
Like map, but returns boolean, so returns new collection with just values that pass the test

```javascript
[1, 2, 3].filter(x => x > 1)
```

returns new collection [2, 3]

concatAll:
Takes a mulch-dimensional collection and flattens it

```javascript
> [ [1], [2, 3], [], [4] ].concatAll()
```

returns [1, 2, 3, 4]

takeUntil:
Listens to its source collection and listens to a stop collection when stop collection fires an item the whole event collection is completed. 


##### Get your top rated films on netflix

```javascript
var getTopRatedFilms = user =>
  user.videoLists.
  map(videoList =>
  videoList.videos.
    filter(video => video.rating === 5.0)).
  concatAll();

getTopRatedFilms(user).
  forEach(film => console.log(film));
```
##### Create a drag event with nearly the same code (mouse drags collection)

```javascript
var getElementDrags = elmt =>
  elmt.mouseDowns.
    map(mouseDown =>
      document.mouseMoves.
        takeUntil(document.mouseUps)).
    concatAll();

getElementDrags(image).
  forEach(pos => image.position = pos); 
```

##### Events to Observables

```javascript
var mouseMoves = 
  Observable.
    fromEvent(element, "mousemove");
```

##### Observable.forEach

```javascript
// subscribe
var subscription = 
  mouseMoves.forEach(console.log);

// unsubscribe
subscription.dispose();\
```

##### Expanded Observable.forEach

```javascript
// subscribe
var subsription = 
  mouseMoves.forEach(
    // next data
    event => console.log(event),
    // error
    error => console.error(error),
    // completed
    () => console.log("done"));

// unsubscribe
subscription.dispose();
```

Don't unsubcribe from Events. Complete them when another event fires.


### Netflix Search
##### Async autocomplete, debounce and handle race condition

```javascript
var searchResultSets = 
  keyPresses.
    throttle(250).
    map(key =>
      getJSON("/searchResults?q=" + input.value).
        retry(3).
        takeUntil(keyPresses)).
    concatAll();

searchResultsSets.forEach(
  resultSet => updateSearchResults(resultSet),
  error => showMessage("the server appears to be down");
```





