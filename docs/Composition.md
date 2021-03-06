
## Overlaid Tracks
[:link: source code](https://github.com/gosling-lang/gosling.js/blob/43626eaf21417bf36128a405dceeaa6ee00d0851/src/core/Gosling.schema.ts#L213)


Gosling enables you to overlay multiple tracks/marks on top of each other. There are two ways for overlaying: (1) specifying all overlaid marks/tracks in `overlay` and (2) overlaying one `track` to previous `track` using `track.overlayOnPreviousTrack: true`.

One `overlay` is composed of an array of objects, in which each object specifies one visual mark. Each visual mark inherits the properties (e.g., `data`, `x`, and `y`) defined in the corresponding track definition unless these properties are redefined in this object.  

```javascript
{
  "tracks": [
    {
      "data": ... , // specify data
      "x": ...,
      "y": ...,
      "color":...,
      "overlay": [
        // point mark and line mark have the same data, x, y, color encoding
        {
          "mark": "line", 
        },
        {
          "mark": "point", 
          // specify the size of point mark
          "size": {"field": "peak", "type": "quantitative", "range": [0, 6]} 
        }
      ]
    }
  ]
}
```

When `track.overlayOnPreviousTrack: true`, a `track` is overlaid on its previous track.
```javascript
{
  "tracks": [
    //first track
    {...},
    // second track that is overlaid on the first track
    {
      ...,
      "overlayOnPreviousTrack": true
    }
  ]
}
```

Try it in the online editor:

[Line chart (line + point)](<https://gosling-lang.github.io/gosling.js/?gist=wangqianwen0418/dd5bd5aa70f2eb68f92f42788f046188>)

[Lollipop plot (bar + point)](<https://gosling-lang.github.io/gosling.js/?gist=wangqianwen0418/d94c24b086bb4f6e5af48af6ebad1ca2>)

[Ideogram (text + rect + triangle)](<https://gosling-lang.github.io/gosling.js/?gist=wangqianwen0418/90cc96ca5f199c78d8985e4c162c6788>)


## Multiple Tracks
Each track of the `tracks` share the same `layout` preporty and are vertically concantenated.
<table>
    <tr>
        <td>  
        <pre>
<code>
{
  "layout": "linear",
  "tracks": [
    {/**track_1**/},
    {/**track_2**/},
    {/**track_3**/}
  ]   
}
</code>
        </pre>
        </td>
        <td>
        <pre>
<code>
{
  "layout": "circular",
  "tracks": [
    {/**track_1**/},
    {/**track_2**/},
    {/**track_3**/}
  ]   
}
</code>
        </pre>
        </td>
    </tr>
    <tr>
        <td> <img src="https://raw.githubusercontent.com/gosling-lang/gosling-docs/master/images/tracks_linear.png" alt="linear tracks" width="400"/> </td>
        <td> <img src="https://raw.githubusercontent.com/gosling-lang/gosling-docs/master/images/tracks_circular.png" alt="circular tracks" width="200"/></td>
    </tr>
</table>

`tracks` compose one single `view`, which has the following properties:
|property|type|description|
|--|--|--|
| layout | string | specify the layout type, either "linear" or "circular" |
| spacing | number | specify the space between tracks|
| static | boolean | whether to disable [Zooming and Panning](https://github.com/gosling-lang/gosling-docs/blob/master/docs/User-Interaction.md#zooming-and-panning), default=false. | 
| assembly | string | currently support "hg38", "hg19", "hg18", "hg17", "hg16", "mm10", "mm9"| 
| xLinkingId | string | specify an ID for [linking multiple views](https://github.com/gosling-lang/gosling-docs/blob/master/docs/User-Interaction.md#linking-views)|
| centerRadius | number | specify the proportion of the radius of the center white space. A number between [0,1], default=0.3|


## Multiple Views
Goslings supports multi-view visualizations. How multiple views are arranged is controlled by the `arrangement` property.
```javascript
{
  "arrangement": "parallel",
  "views": [
    // one view is composed of tracks that share the same layout property (linear or circular)
    {
      "layout": "linear",
      "tracks": [...]
    },
    // One view can have a hierarchical structure. 
    // For example, the view below is composed of two sub-views
    {
      "arrangement": "serial",
      "views": [
        {
          "tracks": [...]
        },
        {
          "tracks": [...]
        }
      ]
    }
  ]
}
```

Gosling supports four types of arrangemet: "parallel", "serial", "vertical", "horizontal".
<img src="https://raw.githubusercontent.com/gosling-lang/gosling-docs/master/images/multi_views.png" alt="arrangement of multiple views" width="700"/> </td>