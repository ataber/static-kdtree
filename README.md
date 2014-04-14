static-kdtree
=============
[kd-trees](http://en.wikipedia.org/wiki/K-d_tree) are a [compact](http://en.wikipedia.org/wiki/Succinct_data_structure) data structure for answering orthogonal range and nearest neighbor queries on higher dimensional point data in linear time.  While they are not as efficient at answering orthogonal range queries as [range trees](http://en.wikipedia.org/wiki/Range_tree), especially in low dimensions, kdtrees consume exponentially less space and support k-nearest neighbor queries.

This library works both in node.js and with [browserify](http://browserify.org/).


**THIS MODULE IS A WORK IN PROGRESS**

# Example

```javascript
var createKDTree = require("static-kdtree")


```

# Install

```sh
npm install static-kdtree
```

# API

```javascript
var createKDTree = require("static-kdtree")
```

By convention, let `n` denote the number of points and `d` denote the dimension of the kdtree.

## Constructor

#### `var kdt = createKDTree(points)`
Creates a kdtree from the given collection of points.

* `points` is either an array of arrays of length `d`, or else an [`ndarray`](https://github.com/mikolalysenko/ndarray) with shape `[n,d]`

**Returns** A kdtree data structure

**Time Complexity** This operation takes O(n log(n))

### `var kdt = createKDTree.deserialze(data)`
Restores a serialized kdtree.

* `data` is a JavaScript object as produced by calling `kdt.serialize`

**Returns** A kdtree data structure equivalent to the one which was serialized.

**Time Complexity** `O(n)`

## Properties

#### `kdt.dimension`
The dimension of the tree

#### `kdt.length`
The number of points in the tree

## Methods

#### `kdt.range(lo, hi, visit)`
Executes an orthogonal range query on the kdtree

* `lo` is a lower bound on the range
* `hi` is an upper bound
* `visit(idx)` is a visitor function which is called once for every point contained in the range `[lo,hi]`. If `visit(idx)` returns any value `!== undefined`, then termination is halted.

**Returns** The last returned value of `visit`

**Time Complexity** `O(n^(1-1/d) + k)`, where `k` is the number of points in the range.

#### `kdt.rnn(point, radius, visit)`
Visit all points contained in the sphere of radius `r` centered at `point`

* `point` is the center point for the query, represented by a length `d` array
* `radius` is the radius of the query sphere
* `visit(idx)` is a function which is called once for every point contained in the ball.  As in the case of `kdt.range`, if `visit(idx)` returns a not undefined value, then iteration is terminated.

**Returns** The last returned value of `visit`

**Time Complexity** `O(n^(1-1/d) + k)`, where `k` is the number of points in the sphere

#### `kdt.knn(point, k[, maxDistance])`
Returns a list of the k closest points to point in the tree.

* `point` is the point which is being queried
* `k` is the number of points to query
* `maxDistance` is an optional parameter which bounds the distance of the returned points. Default is `Infinity`

**Returns** An array of indices of the exact `k` closest points in the tree to the query point.

**Time Complexity** `O((n + k) log(k))`, but may be faster if the points in the tree are uniformly distributed

#### `kdt.serialize()`
Returns a serializable JSON object encoding the state of the kdtree

#### `kdt.dispose()`
Release all resources associated with the kdtree

**Time Complexity** `O(1)`

# Comparison to other kdtree libraries

**TODO**

# Credits
(c) 2014 Mikola Lysenko. MIT License