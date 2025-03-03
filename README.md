# JS MediaTags

The next version of https://github.com/aadsm/JavaScript-ID3-Reader.

Modified version of https://github.com/aadsm/jsmediatags as this supports tracks with multiple genres.

## Donations
A few people have asked me about donations (or even crowdfunding). I would prefer you to consider making a donation to the ["Girls Who Code" NPO](https://www.classy.org/checkout/donation?eid=77372). If you do please send me a message so I can add you as a contributor.

## [Contributors](https://github.com/aadsm/jsmediatags/blob/master/CONTRIBUTORS.md)

## [Contributing](https://github.com/aadsm/jsmediatags/blob/master/CONTRIBUTING.md)

## Current Support

* File Readers
  * NodeJS
  * XMLHttpRequest
  * Blob
  * File
  * Buffers/Arrays
  * React Native
* Tag Readers
  * ID3v1
  * ID3v2 (with unsynchronisation support!)
  * MP4
  * FLAC

## How to use

### NodeJS

Run `npm install jsmediatags --save` to install.

```javascript
// Simple API - will fetch all tags
var jsmediatags = require("jsmediatags");

jsmediatags.read("./music-file.mp3", {
  onSuccess: function(tag) {
    console.log(tag);
  },
  onError: function(error) {
    console.log(':(', error.type, error.info);
  }
});
```

```javascript
// Advanced API
var jsmediatags = require("jsmediatags");

new jsmediatags.Reader("http://www.example.com/music-file.mp3")
  .setTagsToRead(["title", "artist"])
  .read({
    onSuccess: function(tag) {
      console.log(tag);
    },
    onError: function(error) {
      console.log(':(', error.type, error.info);
    }
  });
```

### Browser

Copy the [`dist/jsmediatags.min.js`](https://github.com/aadsm/jsmediatags/blob/master/dist/jsmediatags.min.js) file into your web application directory and include it with a script tag.
This library is also available on cdnjs at https://cdnjs.com/libraries/jsmediatags.
UMD will give you multiple usage options to use it:

```javascript
// As a global Object
var jsmediatags = window.jsmediatags;
```
```javascript
// As a CommonJS Module
var jsmediatags = require("jsmediatags");
```

It supports loading files from remote hosts, Blob and File objects:
```javascript
// From remote host
jsmediatags.read("http://www.example.com/music-file.mp3", {
  onSuccess: function(tag) {
    console.log(tag);
  },
  onError: function(error) {
    console.log(error);
  }
});
```

Note that the URI has to include the scheme (e.g.: https://), as relative URIs are not supported.


```javascript
// From Blob
jsmediatags.read(blob, ...);
```
```javascript
// From File
inputTypeFile.addEventListener("change", function(event) {
  var file = event.target.files[0];
  jsmediatags.read(file, ...);
}, false);
```

You can find more about UMD usage options [here](http://www.forbeslindesay.co.uk/post/46324645400/standalone-browserify-builds).

### React Native

React Native support requires some additional dependencies:

```bash
npm install --save jsmediatags buffer react-native-fs
```

With these dependencies installed, usage with React Native should remain the
same:

```js
const jsmediatags = require('jsmediatags');

new jsmediatags.Reader('/path/to/song.mp3')
  .read({
    onSuccess: (tag) => {
      console.log('Success!');
      console.log(tag);
    },
    onError: (error) => {
      console.log('Error');
      console.log(error);
    }
});

// Or wrap it with a promise
new Promise((resolve, reject) => {
  new jsmediatags.Reader('/path/to/song.mp3')
    .read({
      onSuccess: (tag) => {
        console.log('Success!');
        resolve(tag);
      },
      onError: (error) => {
        console.log('Error');
        reject(error);
      }
  });
})
  .then(tagInfo => {
    // handle the onSuccess return
  })
  .catch(error => {
    // handle errors
  });
```

### Articles

* [Cordova : lire les metadatas des mp3s avec jsmediatags](http://blog.luce.pro/2016/02/28/Phonegap-lire-les-metadatas-des-mp3s-avec-jsmediatags/)

## Documentation

### The Output
This is an example of the object passed to the `jsmediatags.read`'s `onSuccess` callback.

#### ID3v1

```javascript
{
  type: 'ID3',
  version: '1.1',
  tags: {
    title: 'Sorrow Tears and Blood',
    artist: 'Fela Kuti',
    album: 'Opposite People / Sorrow Tears',
    year: '1977',
    comment: '',
    genres: [ 'Ethnic' ],
    genre: 'Ethnic',
    track: 3
  }
}
```

#### ID3v2
```javascript
{
  type: 'ID3',
  version: '2.3.0',
  major: 3,
  revision: 0,
  flags: {
    unsynchronisation: false,
    extended_header: false,
    experimental_indicator: false,
    footer_present: false
  },
  size: 64578,
  tags: {
    title: 'Sorrow Tears and Blood',
    artist: 'Fela Kuti',
    album: 'Opposite People / Sorrow Tears and Blood',
    year: '1977',
    track: '3/4',
    genres: [ 'Afrobeat', 'African' ],
    genre: 'Afrobeat',
    picture: {
      format: 'image/jpeg',
      type: 'Cover (front)',
      description: '',
      data: [Array]
    },
    TALB: {
      id: 'TALB',
      size: 85,
      description: 'Album/Movie/Show title',
      data: 'Opposite People / Sorrow Tears and Blood'
    },
    TPE1: {
      id: 'TPE1',
      size: 23,
      description: 'Lead performer(s)/Soloist(s)',
      data: 'Fela Kuti'
    },
    TPE2: {
      id: 'TPE2',
      size: 23,
      description: 'Band/orchestra/accompaniment',
      data: 'Fela Kuti'
    },
    TCON: {
      id: 'TCON',
      size: 39,
      description: 'Content type',
      data: [Array]
    },
    TPUB: {
      id: 'TPUB',
      size: 27,
      description: 'Publisher',
      data: 'MCA Records'
    },
    TIT2: {
      id: 'TIT2',
      size: 49,
      description: 'Title/songname/content description',
      data: 'Sorrow Tears and Blood'
    },
    TRCK: {
      id: 'TRCK',
      size: 5,
      description: 'Track number/Position in set',
      data: '3/4'
    },
    UFID: {
      id: 'UFID',
      size: 59,
      description: 'Unique file identifier',
      data: [Object]
    },
    TYER: { id: 'TYER', size: 6, description: 'Year', data: '1977' },
    TSOP: {
      id: 'TSOP',
      size: 47,
      description: 'Performer sort order',
      data: 'Kuti, Fela; Africa 70'
    },
    TSO2: {
      id: 'TSO2',
      size: 25,
      description: 'Unknown',
      data: 'Kuti, Fela'
    },
    TXXX: [ [Object], [Object], [Object], [Object], [Object], [Object] ],
    APIC: {
      id: 'APIC',
      size: 61857,
      description: 'Attached picture',
      data: [Object]
    }
  }
}
{
  type: 'MP4',
  ftyp: 'M4A ',
  version: 0,
  tags: {
    '©too': {
      id: '©too',
      size: 99,
      description: 'Encoding Tool',
      data: 'qaac 2.69, CoreAudioToolbox 7.10.9.0, AAC-LC Encoder, TVBR q127, Quality 96'
    },
    trkn: {
      id: 'trkn',
      size: 32,
      description: 'Track Number',
      data: [Object]
    },
    '©ART': { id: '©ART', size: 33, description: 'Artist', data: 'Fela Kuti' },
    aART: {
      id: 'aART',
      size: 33,
      description: 'Album Artist',
      data: 'Fela Kuti'
    },
    '©nam': {
      id: '©nam',
      size: 46,
      description: 'Title',
      data: 'Sorrow Tears and Blood'
    },
    '©alb': {
      id: '©alb',
      size: 64,
      description: 'Album',
      data: 'Opposite People / Sorrow Tears and Blood'
    },
    '©day': { id: '©day', size: 28, description: 'Release Date', data: '1977' },
    covr: {
      id: 'covr',
      size: 61867,
      description: 'Cover Art',
      data: [Object]
    },
    title: 'Sorrow Tears and Blood',
    artist: 'Fela Kuti',
    album: 'Opposite People / Sorrow Tears and Blood',
    year: '1977',
    track: 3,
    picture: { format: 'image/jpeg', data: [Array] },
    genres: [ 'Afrobeat', 'African' ],
    genre: 'Afrobeat'
  }
}
```

#### MP4
```javascript
{
  type: 'MP4',
  ftyp: 'M4A ',
  version: 0,
  tags: {
    '©too': {
      id: '©too',
      size: 99,
      description: 'Encoding Tool',
      data: 'qaac 2.69, CoreAudioToolbox 7.10.9.0, AAC-LC Encoder, TVBR q127, Quality 96'
    },
    trkn: {
      id: 'trkn',
      size: 32,
      description: 'Track Number',
      data: [Object]
    },
    '©ART': { id: '©ART', size: 33, description: 'Artist', data: 'Fela Kuti' },
    aART: {
      id: 'aART',
      size: 33,
      description: 'Album Artist',
      data: 'Fela Kuti'
    },
    '©nam': {
      id: '©nam',
      size: 46,
      description: 'Title',
      data: 'Sorrow Tears and Blood'
    },
    '©alb': {
      id: '©alb',
      size: 64,
      description: 'Album',
      data: 'Opposite People / Sorrow Tears and Blood'
    },
    '©day': { id: '©day', size: 28, description: 'Release Date', data: '1977' },
    covr: {
      id: 'covr',
      size: 61867,
      description: 'Cover Art',
      data: [Object]
    },
    title: 'Sorrow Tears and Blood',
    artist: 'Fela Kuti',
    album: 'Opposite People / Sorrow Tears and Blood',
    year: '1977',
    track: 3,
    picture: { format: 'image/jpeg', data: [Array] },
    genres: [ 'Afrobeat', 'African' ],
    genre: 'Afrobeat'
  }
}
```

#### FLAC
```javascript
{
  type: 'FLAC',
  version: '1',
  tags: {
    title: 'Sorrow Tears and Blood',
    artist: 'Fela Kuti',
    album: 'Opposite People / Sorrow Tears and Blood',
    track: '3/4',
    genres: [ 'Afrobeat', 'African' ],
    genre: 'Afrobeat',
    picture: {
      format: 'image/jpeg',
      type: 'Cover (front)',
      description: '',
      data: [Array]
    }
  }
}
```

The `tags` property includes all tags that were found or specified to be read.
Since each tag type (e.g.: ID3, MP4) uses different tag names for the same type of data (e.g.: the artist name) the most common tags are also available under human readable names (aka shortcuts). In this example, `artist` will point to `TPE1.data`, `album` to `TALB.data` and so forth.

The expected tag object depends on the type of tag read (ID3, MP4, etc.) but they all share a common structure:

```
{
  type: <the tag type: ID3, MP4, etc.>
  tags: {
    <shortcut name>: <points to a tags data>
    <tag name>: {
      id: <tag name>,
      data: <the actual tag data>
    }
  }
}
```

### Shortcuts

These are the supported shortcuts.

* `title`
* `artist`
* `album`
* `year`
* `comment`
* `track`
* `genres`
* `genre` (first element of the genres array)
* `picture`
* `lyrics`

### Multiple genres

FLAC and MP4 supports multiple genre fields.

ID3v1 supports only one genre field and it stores the information in one byte (and uses a predefined list to translate the value back to human readable information), so storing multiple genres is not an option.

ID3v2 supports only one genre field, but up to 16MB in length, so the different genres can be stored joined with a special character. The most common approach is to use the character `65279` as a separator.

Mp3tag does all the required work automatically if the supplied genres are separated with double backslash (`\\`) characters. For example, to have `["Afrobeat", "African"]` in the output of `jsmediatags`, the Genre field of Mp3tag needs to contain the following: `Afrobeat\\African`. This will work with FLAC, MP4 and ID3v2 as well.

### Picture data

The `picture` tag contains an array buffer of all the bytes of the album artwork image as well as the content type of the image. The data can be converted and displayed as an image using:

```javascript
const { data, format } = result.tags.picture;
let base64String = "";
for (const i = 0; i < data.length; i++) {
  base64String += String.fromCharCode(data[i]);
}
img.src = `data:${data.format};base64,${window.btoa(base64String)}`;
```

### HTTP Access Control (CORS)

When using HTTP [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS) requests you need to make sure that the server is configured to receive `If-Modified-Since` and `Range` headers with the request.
This can be configured by returning the `Access-Control-Allow-Headers` HTTP header with the OPTIONS request response.

Similarly, you should also allow for the browser to read the `Content-Length` and `Content-Range` headers. This can be configured by returning the  `Access-Control-Expose-Headers` HTTP header.

In short, the following headers are expected:
```
Access-Control-Allow-Headers: If-Modified-Since, Range
Access-Control-Expose-Headers: Content-Length, Content-Range
```

This library still works without these options configured on the server. However it will download the entire file instead of only the necessary bytes for reading the tags.

### File and Tag Readers

This library uses file readers (MediaFileReader API) to read the file itself and media tag readers (MediaTagReader API) to parse the tags in the file.

By default the library will automatically pick the most appropriate file reader depending on the file location. In the common case this will be the URL or local path where the file is located.

A similar approach is taken for the tag reader. The most appropriate tag reader will be selected depending on the tag signature found in the file.

However, you can specify exactly which file reader or tag reader to use using the advanced API.

New file and tag readers can be implemented by extending the MediaFileReader and MediaTagReader classes. Check the `Development` section down bellow for more information.

### Reference

* `jsmediatags.Reader`
  * `setTagsToRead(tags: Array<string>)` - Specify which tags to read
  * `setFileReader(fileReader: typeof MediaFileReader)` - Use this particular file reader
  * `setTagReader(tagReader: typeof MediaTagReader)` - Use this particular tag reader
  * `read({onSuccess, onError})` - Read the tags.

* `jsmediatags.Config`
  * `addFileReader(fileReader: typeof MediaFileReader)` - Add a new file reader to the automatic detection system.
  * `addTagReader(tagReader: typeof MediaTagReader)` - Add a new tag reader to the automatic detection system.
  * `setDisallowedXhrHeaders(disallowedXhrHeaders: Array<string>)` - Prevent the library from using specific http headers. This can be useful when dealing with CORS enabled servers you don't control.
  * `setXhrTimeoutInSec(timeoutInSec: number)` - Sets the timeout time for http requests. Set it to 0 for no timeout at all. It defaults to 30s.

## Development

Source code uses Flow for type checking meaning that a compilation step is needed to remove all type annotations.
When using this library with NodeJS you can use the runtime compilation that is supported by babel. It will be slightly slower but no compilation step is required.

### NodeJS (With Runtime Compilation)

```javascript
require('babel-core/register');

var NodeFileReader = require('./src/NodeFileReader');
var ID3v2TagReader = require('./src/ID3v2TagReader');
...
```

### NodeJS (With Compiled Code (faster))

Run `npm run build` to generate proper JavaScript code into the `build2` directory.

```javascript
var NodeFileReader = require('./build2/NodeFileReader');
var ID3v2TagReader = require('./build2/ID3v2TagReader');
...
```

Run `npm run watch` to automatically recompile the source code whenever a file is changed.

### Browser

Run `npm run dist` to generate a UMD version of this library that is ready to be used in a browser.

Two packages are created for the browser: `dist/jsmediatags.min.js` and `dist/jsmediatags.js`. One is a minimized version that is meant to be used in production and the other a regular version meant to be used for debugging.

Run `npm run dist-watch` to recompile and browserify the source code whenever a file is changed. This will only regenerate the `dist/jsmediatags.js` file.

### New File Readers

Extend the `MediaFileReader` class to implement a new file reader. Methods to implement are:

* init
* loadRange
* getBytesLoaded
* getByteAt

Current Implementations:
* [NodeFileReader](https://github.com/aadsm/jsmediatags/blob/master/src/NodeFileReader.js) (NodeJS)
* [XhrFileReader](https://github.com/aadsm/jsmediatags/blob/master/src/XhrFileReader.js) (Browser and NodeJS)
* [BlobFileReader](https://github.com/aadsm/jsmediatags/blob/master/src/BlobFileReader.js) (Blob and File)


### New Tag Readers

Extend the `MediaTagReader` class to implement a new tag reader. Methods to implement are:

* getTagIdentifierByteRange
* canReadTagFormat
* \_loadData
* \_parseData

Current Implementations:
* [ID3v1TagReader](https://github.com/aadsm/jsmediatags/blob/master/src/ID3v1TagReader.js)
* [ID3v2TagReader](https://github.com/aadsm/jsmediatags/blob/master/src/ID3v2TagReader.js)
* [MP4TagReader](https://github.com/aadsm/jsmediatags/blob/master/src/MP4TagReader.js)

### Unit Testing

Jest is the framework used. Run `npm test` to execute all the tests.

## JavaScript-ID3-Reader
If you want to migrate your project from [JavaScript-ID3-Reader](https://github.com/aadsm/JavaScript-ID3-Reader) to `jsmediatags` use the following guiding examples:

### All tags
**JavaScript-ID3-Reader:**
```javascript
ID3.loadTags("filename.mp3", function() {
  var tags = ID3.getAllTags("filename.mp3");
  alert(tags.artist + " - " + tags.title + ", " + tags.album);
});
```
**jsmediatags:**
```javascript
jsmediatags.read("filename.mp3", {
  onSuccess: function(tag) {
    var tags = tag.tags;
    alert(tags.artist + " - " + tags.title + ", " + tags.album);
  }
});
```

### Specific tags
**JavaScript-ID3-Reader:**
```javascript
ID3.loadTags("filename.mp3", function() {
  var tags = ID3.getAllTags("filename.mp3");
  alert(tags.COMM.data + " - " + tags.TCON.data + ", " + tags.WXXX.data);
},
{tags: ["COMM", "TCON", "WXXX"]});
```
**jsmediatags:**
```javascript
new jsmediatags.Reader("filename.mp3")
  .setTagsToRead(["COMM", "TCON", "WXXX"])
  .read({
    onSuccess: function(tag) {
      var tags = tag.tags;
      alert(tags.COMM.data + " - " + tags.TCON.data + ", " + tags.WXXX.data);
    }
  });
```
### Error handling
**JavaScript-ID3-Reader:**
```javascript
ID3.loadTags("http://localhost/filename.mp3", function() {
  var tags = ID3.getAllTags("http://localhost/filename.mp3");
  alert(tags.comment + " - " + tags.track + ", " + tags.lyrics);
},
{
  tags: ["comment", "track", "lyrics"],
  onError: function(reason) {
    if (reason.error === "xhr") {
      console.log("There was a network error: ", reason.xhr);
    }
  }
});
```
**jsmediatags:**
```javascript
new jsmediatags.Reader("filename.mp3")
  .setTagsToRead(["comment", "track", "lyrics"])
  .read({
    onSuccess: function(tag) {
      var tags = tag.tags;
      alert(tags.comment + " - " + tags.track + ", " + tags.lyrics);
    },
    onError: function(error) {
      if (error.type === "xhr") {
        console.log("There was a network error: ", error.xhr);
      }
    }
  });
```

## Goals

* Improve the API of JavaScript-ID3-Reader
* Improve the source code with readable code and Flow annotated types
* Have unit tests
* Support NodeJS
