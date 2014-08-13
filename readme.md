# tracking-script

Track what a user does on a site in csv-format

## Example

Running `node example/server.js` will start an example script on port 1234 that does POST requests whenever an event is tracked, and that event is then printed to the terminal

## Usage

```js
var xhr = require('xhr')

require('./track')(function (csv, callback) {
  xhr({
      method: 'POST'
    , body: csv
    , uri: '/track'
  }, callback)
})
```

The callback to './track' gets called everytime a trackable event occur. _csv_ is the data about the event (see [data-format](#data-format) for details)

## Data format

### Generic Headers

```
event,windowWidth,windowHeight,scollX,scollY,location,offset,userAgent,referrer
```

The data always has columns corresponding to these headers:

* __event__ Describes what event that has occured. Is one of the following:
 * __load__ Emitted when the page has loaded (_window.onload_)
 * __resize__ Emitted everytime a user resize the window (_window.onresize_). All resizing within 500ms are tracked as one resize-event.
 * __scroll__ Emitted everytime a user scroll. All scrolling within 500ms is tracked as one scoll-event.
 * __initial visibility__ Event describing if the page was loaded visible or not. This event is only emitted if the user is on a modern browser.
 * __blur__ Emitted when a user changes windows/tab (_window.onblur_)
 * __focus__ Emitted when a user open the window/tab (_window.onfocus_)
 * __change__ Emitted when a user changes a form (_document.onchange_)
 * __click__ Emitted when a user clicks on the page
* __windowWidth__ The width of the users window (Number in px)
* __windowHeight__ The height of the users window (Number in px)
* __scrollX__ How far the user has scrolled (horizontally)
* __scrollY__ How far the user has scrolled (vertically)
* __location__ The page the user is on (_window.location_)
* __offset__ Time (in ms) that has gone by since tracking was initiated
* __userAgent__ The useragent the user has (_navigator.userAgent_)
* __referrer__ The referrer header (_document.referrer_)

### click-event

```
event,windowWidth,windowHeight,scollX,scollY,location,offset,userAgent,referrer,path,clickX,clickY,href,target
```
The following headers are specific to a `click` event:

* __path__ The css-path describing the DOM-element that was clicked
* __clickX__ The x-coordinate on the page that was clicked
* __clickY__ The y-coordinate on the page that was clicked
* __href__ The href-attribute on the DOM-element that was clicked
* __target__ the target-attribute on teh DOM-element that was clicked

### initial visibility

```
event,windowWidth,windowHeight,scollX,scollY,location,offset,userAgent,referrer,visibility
```

The following header is specific to a `initial visibility` event:

*__visibility__ String describing if the page was visible or not. Can be one of `visible` or `hidden`

### change

```
event,windowWidth,windowHeight,scollX,scollY,location,offset,userAgent,referrer,path,name
```

The following headers are specific to a `change` event:

* __path__ The css-path describing the DOM-element that was changed
* __name__ The name-attribute on the DOM-element that was changed
