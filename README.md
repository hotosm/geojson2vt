# geojson2vt

Generates Mapbox Vector Tiles from geojson files. It will output a directory containing the vector tiles in the pbf format with gzip compression.

## How to Use

### Install

`npm install @hotosm/geojson2vt`

### Using

`geojson2vt` takes a config object with the GeoJSONs to encode and other options, and builds the vector tiles pyramid in the specified output directory.

```
var fs = require('fs');
var geojson2vt = require('geojson2vt');

var options = {
  layers: {
    layer0: JSON.parse(fs.readFileSync('bus_routes.geojson', "utf8")),
    layer1: JSON.parse(fs.readFileSync('stops.geojson', "utf8"))
  },
  rootDir: 'tiles',
  bbox: [40.426042,-74.599228,40.884448,-73.409958], // [south,west,north,east]
  zoom : {
    min : 8,
    max : 18,
  }
};
// build the static tile pyramid
geojson2vt(options);
```

## Options

`layers` - Object<string,Object> (required) - GeoJSONs to create a vector tileset from. Keys are the layer names that will be used to access data from the respective GeoJSON when displaying data from the MVT.

`rootDir` - string (required) - the filepath of the directory that will be the root of the file pyramid.  It will be created if it doesn't exist.

`bbox` - array (required) - array of lat/lon bounds like `[s,w,n,e]`

`zoom` - object (required) - object with `min` and `max` properties for the desired zoom levels in the tile pyramid

## Backwards compatibility

Instead of providing a single config object, you can provide two arguments: a geoJson and config object without a `layers` property, but instead with a `layerName` property for the name for the imported geoJson in the MVT.
