express-static-zip
==================

Serve static files out of a ZIP without unpacking.

## About

  Adding client-side dependency libraries to the repository is a good practice, but
  there are huge ones like [dojo](http://dojotoolkit.org) and some proprietary ones like
  [Arcgis JS API](http://js.arcgis.com) containing thousands of small files, making it a
  mess. You either go with them, making the repo some ten times slower, or use some 
  (probably internally) hosted ones, messing with versions and switching paths.
  
  This module provides means to serving such libraries or arbitrary static file packages
  right out of a ZIP without unpacking. Zipped library goes well with the repos even being
  several tens of megabytes in size. Zipped library doesn't pollute your repo with its contents.

  You can add a classic `express.static` for production environment, with an unzip deployment
  script.

## Example

  Provided you have a `biglib.zip` in your root server folder, and serving library on url root:

```js
var express = require('express');
var staticZip = require('express-static-zip');

var app = express();

app.use(staticZip('./biglib.zip'));

app.listen(process.env.PORT || 3000);
```

  Now you are free to `GET` _/some/files/inside/zip.txt_

## Options

```js
app.use(staticZip('./biglib.zip', {
  zipRoot: "a-folder/";  // Default: ""
                         // Use a directory inside ZIP file as the root
}));
```

## Additional info

  Zip compressed content is loaded to memory synchronously at startup. It doesn't take long, some
  50-300ms for a 20Mb JS lib with some 15k files.
  
  Files are uncompressed and cached in-memory upon request.
