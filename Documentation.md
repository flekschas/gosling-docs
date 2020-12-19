- [Overview](#overview)
- [Data](#data)
    - [Multivec (HiGlass)](#multivec-higlass)
    - [BED (HiGlass)](#bed-higlass)
    - [BED](#bed)
    - [Vector (HiGlass)](#vector-higlass)
    - [CSV](#csv)
- [Mark](#mark)
  - [Type](#type)
    - [Point](#point)
    - [Line](#line)
    - [Area](#area)
    - [Bar](#bar)
    - [Rect](#rect)
    - [Text](#text)
    - [Link](#link)
    - [Rule](#rule)
    - [Triangle](#triangle)
    - [Brush](#brush)
    - [Glyph](#glyph)
  - [Visual channel](#visual-channel)
    - [x](#x)
    - [xe](#xe)
    - [y](#y)
    - [ye](#ye)
    - [row](#row)
    - [color](#color)
    - [stroke](#stroke)
    - [strokeWidth](#strokewidth)
    - [opacity](#opacity)
    - [style](#style)
    - [superpose](#superpose)
    - [innerRadius](#innerradius)
    - [outerRadius](#outerradius)
- [Track](#track)
  - [Layout](#layout)
  - [style](#style-1)
  - [Arrangement](#arrangement)
    - [Grid-based arrangement](#grid-based-arrangement)
    - [Superposition](#superposition)
- [Interactions](#interactions)
  - [Linking Views](#linking-views)
  - [Zooming and Panning](#zooming-and-panning)
  - [Semantic Zoom](#semantic-zoom)
  - [Tooltip](#tooltip)



# Overview

# Data

### Multivec (HiGlass)

```json
...
"data": {
    "url": "https://resgen.io/api/v1/tileset_info/?d=UvVPeLHuRDiYA3qwFlm7xQ",
    "type": "tileset"
},
"metadata": {
    "type": "higlass-multivec",
    "row": "sample",
    "column": "position",
    "value": "peak",
    "categories": ["sample 1", "sample 2", "sample 3", "sample 4"]
},
...
```
### BED (HiGlass)
```json
...
"data": {
    "url": "https://higlass.io/api/v1/tileset_info/?d=OHJakQICQD6gTD7skx4EWA",
    "type": "tileset"
},
"metadata": {
    "type": "higlass-bed",
    "genomicFields": [
        {"index": 1, "name": "start"},
        {"index": 2, "name": "end"}
    ],
    "valueFields": [
        {"index": 5, "name": "strand", "type": "nominal"},
        {"index": 3, "name": "name", "type": "nominal"}
    ],
    "exonIntervalFields": [
        {"index": 12, "name": "start"},
        {"index": 13, "name": "end"}
    ]
},
...
```
### BED
### Vector (HiGlass)
### CSV

# Mark
[source code](https://github.com/sehilyi/geminid/tree/master/src/core/mark)

Marks (e.g., points, lines) are the basic graphical elements of a visualization.
The core of a visualization is to bind selected **data fields** to the **visual channels** (e.g., size, color, position) of a chosen **mark type**.

## Type  
The `mark` property of a track is defined by a string that describe the mark type.
```javascript
// an example

{
    "tracks":[{
        "mark":"rect",
        ... // other track properties
    }],
    ... // other visualization properties
}
```
Geminid supports the following primitive `mark` types: `point`, `line`, `area`, `bar`, `rect`, `text`, `link`, `rule`, `triangle`. Composite mark (i.e., glyph) is also supported through the `"superpose"` property.


### Point
[source code](https://github.com/sehilyi/geminid/blob/master/src/core/mark/point.ts)

<img src="https://github.com/sehilyi/geminid/wiki/images/point_example.png" width="800" alt="point_example">  

[open the example in online editor]()

```javascript
// example
{
    "tracks":[{
        "data": {
            "url": ...,
            "type": ...
        },
        // mark type
        "mark": "point",
        // mark visual channels
        "x": {
            "field": "position", // data field
            "type": "genomic", // type of data field
            "axis": "top"
        },
        "y": {
            "field": "peak", 
            "type": "quantitative"
        },
        ... // other encodings and styles
    }]
}
```
### Line
### Area
### Bar
### Rect
### Text
### Link
### Rule

### Triangle
[source code](https://github.com/sehilyi/geminid/blob/master/src/core/mark/triangle.ts)  
Support three types of triangle marks: `triangle-l`, `triangle-r`, `triangle-d`

### Brush
### Glyph
<!-- This will cover the `superpose`, mostly in the perspective of making a glyph (e.g., gene annotation) -->

## Visual channel  
The visual appearance of a mark is controlled by a set of visual channels (e.g., size, position, color hue), which are binded with data fields.
Different marks have different visual channels.
Overall,
Geminid supports the following general visual channels:
`x`, `y`, `xe`, `ye`, `color`, `size`.

| mark channel property | type    | description                                                                                                                                         |
| --------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| field                 | string  | the data field name                                                                                                                                 |
| type                  | string  | specify type of the data field. support `genomic`, `nominal`, `quantitative`                                                                        |
| aggregate             | string  | support `max`, `min`, `mean`, `bin`, `count`                                                                                                        |
| domain                |         |                                                                                                                                                     |
| range                 |         |                                                                                                                                                     |
| axis                  | string  | This property is only used for visual channels `x` and `y`. It specifies the position of the axis. Support `none`, `top`, `bottom`, `left`, `right` |
| baseline              |         |                                                                                                                                                     |
| legend                | boolean | whether show the legend                                                                                                                             |



```javascript
// an example configuration for a line chart (x and y are encoded)

{
    "tracks":[{
      "data": {
        "url": ...,
        "type": ...
      },
      "mark": "line",
      // below are the visual channels of the line
      "x": {
        "field": "position",
        "type": "genomic",
        "domain": {"chromosome": "1", "interval": [1, 3000500]},
        "axis": "top"
      },
      "y": {
          "field": "peak", 
          "type": "quantitative"
          }
    }]
}

```


### x
### xe
### y
### ye
### row



<!-- a little bit confusing that x, y indicate both the axes and the encoding of the mark, even though vega lite employs the same strategy -->

<!-- Another question, how can I rotate a chart, for example, the area chart in basic marks, 90 degree? (maybe this is a rare case in gemonic visualization?)
 -->

---

### color
<!-- I didn't see the legend (when set legend: true) of color when {"type": "quantitative"} -->
### stroke
### strokeWidth
### opacity
<!-- will it be better if we merge stroke, strokeWidth, background, opacity into a style option? -->
### style

---

### superpose
overlay another track on the original track. 
Superpose share the same options as the original track unless it is specified.

---

only useful when `{"type": "circular"}`

### innerRadius
### outerRadius

---

# Track
## Layout
[source code](https://github.com/sehilyi/geminid/blob/00a7b5c6a95528dbabdb2444ef469a1448689d3b/src/core/geminid.schema.ts#L22)
This determines the layout of a track, either `circular` or `linear`.

| property | type     | description                                                       |
| -------- | -------- | ----------------------------------------------------------------- |
| layout   | `string` | **Required**, specify the type of layout (`linear` or `circular`) |

## style

## Arrangement
`object`  

### Grid-based arrangement
[source code](https://github.com/sehilyi/geminid/blob/00a7b5c6a95528dbabdb2444ef469a1448689d3b/src/core/geminid.schema.ts#L20)
specify the grid arrangement of multiple tracks

| property                | type                        | description                                                                                                                                    |
| ----------------------- | --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| arrangement.direction   | `string`                    | **Required**, the layout direction of tracks (`vertical`   or `horizontal`)                                                                    |
| arrangement.wrap        | `number`                    | specify the number of tracks at each row (when `direction:horizontal`) or at each column (when `direction:vertical`). default value = infinite |
| arrangement.rowSizes    | `number` \| `Array<number>` |                                                                                                                                                |
| arrangement.rowGaps     | `number` \| `Array<number>` |                                                                                                                                                |
| arrangement.columnSizes | `number` \| `Array<number>` |                                                                                                                                                |
| arrangement.columnGaps  | `number` \| `Array<number>` |                                                                                                                                                |

<img src="https://github.com/sehilyi/geminid/wiki/images/layout_demo.png" alt="layout demo" width="400">

<!-- is it possible that several tracks under one layout have different type (linear and circular) -->

### Superposition
[source code](https://github.com/sehilyi/geminid/blob/00a7b5c6a95528dbabdb2444ef469a1448689d3b/src/core/geminid.schema.ts#L213)

# Interactions



## Linking Views
[source code](ttps://github.com/sehilyi/geminid/blob/00a7b5c6a95528dbabdb2444ef469a1448689d3b/src/core/geminid.schema.ts#L328)


## Zooming and Panning
[source code](https://github.com/sehilyi/geminid/blob/00a7b5c6a95528dbabdb2444ef469a1448689d3b/src/core/geminid.schema.ts#L7)

## Semantic Zoom
[source code](https://github.com/sehilyi/geminid/blob/00a7b5c6a95528dbabdb2444ef469a1448689d3b/src/core/geminid.schema.ts#L203)

## Tooltip
[source code](https://github.com/sehilyi/geminid/blob/00a7b5c6a95528dbabdb2444ef469a1448689d3b/src/core/geminid.schema.ts#L168)