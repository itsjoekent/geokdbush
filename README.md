## geokdbush

A geographic extension for [kdbush](https://github.com/mourner/kdbush),
the fastest static spatial index for points in JavaScript.

It implements fast [nearest neighbors](https://en.wikipedia.org/wiki/Nearest_neighbor_search) queries
for locations on Earth, taking Earth curvature and date line wrapping into account.
Similar to [sphere-knn](https://github.com/darkskyapp/sphere-knn), but orders of magnitude faster.

### Example

```js
var kdbush = require('kdbush');
var geokdbush = require('geokdbush');

var index = kdbush(points, (p) => p.lon, (p) => p.lat);

var nearest = geokdbush.around(index, -119.7051, 34.4363, 1000);
```

### API

#### geokdbush.around(index, longitude, latitude[, maxResults, maxDistance, filterFn])

Returns an array of the closest points from a given location in order of increasing distance.

- `index`: [kdbush](https://github.com/mourner/kdbush) index.
- `longitude`: query point longitude.
- `latitude`: query point latitude.
- `maxResults`: (optional) maximum number of points to return (`Infinity` by default).
- `maxDistance`: (optional) maximum distance to search within (`Infinity` by default).
- `filterFn`: (optional) a function to filter the results with.

### Performance

`geokdbush` is incredibly fast.
The results below were obtained by running `npm run bench`
on Node v7.7.2, Macbook Pro Retina 15 mid-2012.

benchmark | geokdbush | sphere-knn | naive
--- | ---: | ---: | ---:
index 138398 points | 69ms | 1027ms | n/a
query 1000 closest | 5ms | 5ms | 155ms (sort all)
query 50000 closest | 26ms | 391ms | 155ms
query all 138398 | 63ms | 29.9s | 155ms
1000 queries of 1 | 21ms | 169ms | 18.4s (naive loop)
