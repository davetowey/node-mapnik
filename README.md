# node-mapnik

Bindings to [Mapnik](http://mapnik.org) for [node](http://nodejs.org).

[![NPM](https://nodei.co/npm/mapnik.png?downloads=true&downloadRank=true)](https://nodei.co/npm/mapnik/)

[![Build Status](https://secure.travis-ci.org/mapnik/node-mapnik.png)](https://travis-ci.org/mapnik/node-mapnik)
[![Build status](https://ci.appveyor.com/api/projects/status/g7f7ow5rv6ac1wt7)](https://ci.appveyor.com/project/springmeyer/node-mapnik)

## Usage

Render a map from a stylesheet:

```js
var mapnik = require('mapnik');
var fs = require('fs');

// register fonts and datasource plugins
mapnik.register_default_fonts();
mapnik.register_default_input_plugins();

var map = new mapnik.Map(256, 256);
map.load('./test/stylesheet.xml', function(err,map) {
    if (err) throw err;
    map.zoomAll();
    var im = new mapnik.Image(256, 256);
    map.render(im, function(err,im) {
      if (err) throw err;
      im.encode('png', function(err,buffer) {
          if (err) throw err;
          fs.writeFile('map.png',buffer, function(err) {
              if (err) throw err;
              console.log('saved map image to map.png');
          });
      });
    });
});
```

Convert a jpeg image to a png:

```js
var mapnik = require('mapnik');
new mapnik.Image.open('input.jpg').save('output.png');
```

Convert a shapefile to GeoJSON:

```js
var mapnik = require('mapnik');
mapnik.register_datasource(path.join(mapnik.settings.paths.input_plugins,'shape.input'));
var ds = new mapnik.Datasource({type:'shape',file:'test/data/world_merc.shp'});
var featureset = ds.featureset()
var geojson = {
  "type": "FeatureCollection",
  "features": [
  ]
}
var feat = featureset.next();
while (feat) {
    geojson.features.push(JSON.parse(feat.toJSON()));
    feat = featureset.next();
}
fs.writeFileSync("output.geojson",JSON.stringify(geojson,null,2));
```

For more sample code see [the tests](./test) and [sample code](https://github.com/mapnik/node-mapnik-sample-code).

## Depends

* Node v0.10.x or v0.11.x

## Installing

Just do:

    npm install mapnik@3.x

Note: This will install the latest node-mapnik 3.x series, which is recommended. There is also an [1.x series](https://github.com/mapnik/node-mapnik/tree/1.x) which maintains API compatibility with Mapnik 2.3.x and 2.2.x and a [v0.7.x series](https://github.com/mapnik/node-mapnik/tree/v0.7.x) which is not recommended unless you need to support Mapnik 2.1 or older.

By default, binaries are provided for:

 - 64 bit OS X 10.9, 64 bit Linux (>= Ubuntu Trusty), and 64/32 bit Windows
 - Node v0.10.x and Node v0.11.13

On those platforms no external dependencies are needed.

Binaries started being provided at node-mapnik >= 1.4.2 for OSX and Linux and at 1.4.8 for Windows.

NOTE: Windows binaries for the 3.x series require the Visual C++ Redistributable Packages for Visual Studio 2014:

  - https://mapnik.s3.amazonaws.com/dist/dev/VS-2014-runtime/vcredist_x64.exe
  - https://mapnik.s3.amazonaws.com/dist/dev/VS-2014-runtime/vcredist_x86.exe

And the 1.x series require the Visual C++ Redistributable Packages for Visual Studio 2013:

 - http://www.microsoft.com/en-us/download/details.aspx?id=40784

Other platforms will fall back to a source compile: see [Source Build](#source-build) for details.

## Source Build

To build from source you need:

 - Mapnik >= v3.x
 - Protobuf >= 2.3.0 (protoc and libprotobuf-lite)

Install Mapnik using the instructions at: https://github.com/mapnik/mapnik/wiki/Mapnik-Installation

Confirm that the `mapnik-config` program is available and on your $PATH.

Then run:

    npm install mapnik --build-from-source

## Using node-mapnik from your node app

To require node-mapnik as a dependency of another package put in your package.json:

    "dependencies"  : { "mapnik":"*" } // replace * with a given semver version string

## Tests

To run the tests do:
  
    npm test

## License

  BSD, see LICENSE.txt
