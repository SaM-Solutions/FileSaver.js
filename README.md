This repo contains sources of original [eligrey/FileSaver.js](https://github.com/eligrey/FileSaver.js) repo with `saveAs()` implementation
and also sources from [ChenWenBrian/FileSaver.js](https://github.com/ChenWenBrian/FileSaver.js) fork which introduces `saveTextAs()` - solution for IE 9- browsers.

---

FileSaver.js
============

FileSaver.js implements the HTML5 W3C `saveAs()` FileSaver interface in browsers that do
not natively support it. There is a [FileSaver.js demo][1] that demonstrates saving
various media types.

FileSaver.js is the solution to saving files on the client-side, and is perfect for
webapps that need to generate files, or for saving sensitive information that shouldn't be
sent to an external server.

Looking for `canvas.toBlob()` for saving canvases? Check out
[canvas-toBlob.js][2] for a cross-browser implementation.

Supported browsers
------------------

| Browser        | Constructs as | Filenames    | Max Blob Size | Dependencies |
| -------------- | ------------- | ------------ | ------------- | ------------ |
| Firefox 20+    | Blob          | Yes          | 800 MiB       | None         |
| Firefox < 20   | data: URI     | No           | n/a           | [Blob.js](https://github.com/eligrey/Blob.js) |
| Chrome         | Blob          | Yes          | [500 MiB][3]  | None         |
| Chrome for Android | Blob      | Yes          | [500 MiB][3]  | None         |
| IE 10+         | Blob          | Yes          | 600 MiB       | None         |
| IE < 10        | text/html     | Yes          | n/a           | None         |
| Opera 15+      | Blob          | Yes          | 500 MiB       | None         |
| Opera < 15     | data: URI     | No           | n/a           | [Blob.js](https://github.com/eligrey/Blob.js) |
| Safari 6.1+*   | Blob          | No           | ?             | None         |
| Safari < 6     | data: URI     | No           | n/a           | [Blob.js](https://github.com/eligrey/Blob.js) |

Feature detection is possible:

```js
try {
    var isFileSaverSupported = !!new Blob;
} catch (e) {}
```

### IE < 10

saveTextAs() will help you to save HTML documents or text file in IE < 10 without Flash-based
polyfills. However, only text based files could be saved in IE < 10, that means canvas will not be
supported.

### Safari 6.1+

Blobs may be opened instead of saved sometimes—you may have to direct your Safari users to manually
press <kbd>⌘</kbd>+<kbd>S</kbd> to save the file after it is opened. Using the `application/octet-stream` MIME type to force downloads [can cause issues in Safari](https://github.com/eligrey/FileSaver.js/issues/12#issuecomment-47247096).

### iOS

saveAs must be run within a user interaction event such as onTouchDown or onClick; setTimeout will prevent saveAs from triggering. Due to restrictions in iOS saveAs opens in a new window instead of downloading, if you want this fixed please [tell Apple](https://bugs.webkit.org/show_bug.cgi?id=102914) how this bug is affecting you.

Syntax
------

Save Text File:
```js
boolean saveTextAs(in textContent, in fileName, in charset)
```
Save File(HTML 5):
```js
FileSaver saveAs(in Blob data, in DOMString filename)
```

Examples
--------

### Saving text(All Browsers)

```js
saveTextAs("Hi,This,is,a,CSV,File", "test.csv");
saveTextAs("<div>Hello, world!</div>", "test.html");
```

For IE < 10, available file extensions are ".htm/.html/.txt", any other text based file will be appended with ".txt" file extension automatically. For example, "test.csv" will be saved as "test_csv.txt".

### Saving text(HTML 5)

```js
var blob = new Blob(["Hello, world!"], {type: "text/plain;charset=utf-8"});
saveAs(blob, "hello world.txt");
```

The standard W3C File API [`Blob`][4] interface is not available in all browsers.
[Blob.js][5] is a cross-browser `Blob` implementation that solves this.

### Saving a canvas(HTML 5)

```js
var canvas = document.getElementById("my-canvas"), ctx = canvas.getContext("2d");
// draw to canvas...
canvas.toBlob(function(blob) {
    saveAs(blob, "pretty image.png");
});
```

Note: The standard HTML5 `canvas.toBlob()` method is not available in all browsers.
[canvas-toBlob.js][6] is a cross-browser `canvas.toBlob()` that polyfills this.


![Tracking image](https://in.getclicky.com/212712ns.gif)

  [1]: http://eligrey.com/demos/FileSaver.js/
  [2]: https://github.com/eligrey/canvas-toBlob.js
  [3]: https://code.google.com/p/chromium/issues/detail?id=375297
  [4]: https://developer.mozilla.org/en-US/docs/DOM/Blob
  [5]: https://github.com/eligrey/Blob.js
  [6]: https://github.com/eligrey/canvas-toBlob.js

Contributing
------------

The `FileSaver.js` distribution file is compiled with Uglify.js like so:

```bash
uglifyjs FileSaver.js --comments /@source/ > FileSaver.min.js
```

Please make sure you build a production version before submitting a pull request.

Bower Installation
------------------

Please see the [this repo](http://github.com/Teleborder/FileSaver.js) for a bower-compatible fork of FileSaver.js, available under the package name `file-saver.js`.
