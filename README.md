# listtojson
An npm module for node.js to convert HTML lists to JSON objects. Basically ordered and
unordered lists are the same. Just the browser treats them differently in rendering bullets 
or numbers I decided to not differenciate between them and just provide a package for all of
them. 

This package can be passed the markup for a single list as a string, a fragment of HTML 
or an entire page or just a URL (with an optional callback function; promises also supported).

The response is always an array. Every array entry in the response represents a list found 
on the page (in same the order they were found in the HTML).


## Basic Usage

Install via npm

```
npm install listtojson
```

### Remote (`convertUrl`)

```javascript
'use strict';

const listtojson = require('listtojson');

listtojson.convertUrl(
    'https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes',
    function(listsAsJson) {
        console.log(listsAsJson[1]);
    }
);

```

### Local (`convert`)
Have a look in the tests folder.

```javascript
'use strict';

const listtojson = require('../lib/listtojson');
const fs = require('fs');
const path = require('path');

const html = fs.readFileSync(path.resolve(__dirname, '../test/lists.html'), {encoding: 'UTF-8'});
const converted = listtojson.convert(html);

console.log(converted);
```

## Options

### request (only `convertUrl`)
If you need to get data from a remote server to pass it to the parser you can call `listtojson.convertUrl`.
When working behind a proxy you can pass any request-options (proxy, headers,...) by adding a request
object to the options passed to `convertUrl`.
for more information on how to configure request please have a look at: 
[https://github.com/request/request](https://github.com/request/request)

``` javascript
listtojson.convertUrl('https://www.timeanddate.com/holidays/ireland/2017', {
    useFirstRowForHeadings: true,
    request: {
        proxy: 'http://proxy:8080'
    }
});
```
### containsClasses
Array of classes to find a specific list using this css class. Default is 'null/undefined'.

### id
The id of the list which is to be fetched provided as a string. Default is 'null/undefined'.
