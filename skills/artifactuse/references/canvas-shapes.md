# Canvas Shapes Reference

Complete reference for all shape types in Artifactuse.

## Common Properties (All Shapes)

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `x` | number | 0 | X position (pixels) |
| `y` | number | 0 | Y position (pixels) |
| `rotation` | number | 0 | Rotation (degrees) |
| `opacity` | number | 100 | Opacity (0-100) |
| `fillColor` | string | - | Fill color (hex or "transparent") |
| `color` | string | "#1e1e1e" | Stroke color |
| `lineWidth` | number | 2 | Stroke width (pixels) |
| `tiltX` | number | 0 | 3D tilt forward/backward in radians (±1.047 max ≈ ±60°) |
| `tiltY` | number | 0 | 3D tilt left/right in radians (±1.047 max ≈ ±60°) |
| `visible` | boolean | true | Whether shape is visible |
| `scaleX` | number | 1 | Horizontal scale (-1 to flip horizontally) |
| `scaleY` | number | 1 | Vertical scale (-1 to flip vertically) |

### Sketchy Mode Properties

These properties enable hand-drawn style rendering (Rough.js). Only works on: rect, circle, ellipse, diamond, triangle, line, arrow.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `renderMode` | string | "smooth" | `"smooth"` or `"sketchy"` |
| `roughness` | number | 1 | Sketchy intensity (0-2) |
| `seed` | number | random | Random seed for deterministic sketchy output |

### Stroke Style Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `strokeStyle` | string | "solid" | `"solid"`, `"dashed"`, or `"dotted"` |
| `edgeStyle` | string | "sharp" | `"sharp"` or `"round"` (affects line caps/joins) |

## Rectangle

`x, y` is the **top-left corner**.

```json
{
  "type": "rect",
  "x": 100,
  "y": 100,
  "width": 200,
  "height": 150,
  "fillColor": "#3498db",
  "color": "#2980b9",
  "lineWidth": 2,
  "cornerRadius": 10
}
```

### Individual Corner Radii

```json
{
  "type": "rect",
  "x": 100,
  "y": 100,
  "width": 200,
  "height": 150,
  "fillColor": "#3498db",
  "cornerRadii": {
    "tl": 20,
    "tr": 10,
    "br": 20,
    "bl": 10
  }
}
```

## Circle

`x, y` is the **center point**.

```json
{
  "type": "circle",
  "x": 200,
  "y": 200,
  "radius": 50,
  "fillColor": "#e74c3c",
  "color": "#c0392b",
  "lineWidth": 2
}
```

## Ellipse

`x, y` is the **center point**.

```json
{
  "type": "ellipse",
  "x": 200,
  "y": 200,
  "radiusX": 80,
  "radiusY": 50,
  "fillColor": "#9b59b6"
}
```

## Diamond

`x, y` is the **center point**.

```json
{
  "type": "diamond",
  "x": 200,
  "y": 200,
  "width": 100,
  "height": 100,
  "fillColor": "#f39c12"
}
```

You can also use `size` for equal width/height:

```json
{
  "type": "diamond",
  "x": 200,
  "y": 200,
  "size": 100,
  "fillColor": "#f39c12"
}
```

## Triangle

`x, y` is the **center point**.

```json
{
  "type": "triangle",
  "x": 200,
  "y": 200,
  "size": 100,
  "fillColor": "#1abc9c"
}
```

Or define custom vertices:

```json
{
  "type": "triangle",
  "x1": 200,
  "y1": 100,
  "x2": 300,
  "y2": 250,
  "x3": 100,
  "y3": 250,
  "fillColor": "#1abc9c"
}
```

## Line

Uses `x1, y1` (start) and `x2, y2` (end).

```json
{
  "type": "line",
  "x1": 100,
  "y1": 100,
  "x2": 300,
  "y2": 200,
  "color": "#34495e",
  "lineWidth": 3
}
```

### Curved Line

Add a `controlPoint` for quadratic bezier curve:

```json
{
  "type": "line",
  "x1": 100,
  "y1": 100,
  "x2": 300,
  "y2": 100,
  "controlPoint": { "x": 200, "y": 50 },
  "color": "#34495e",
  "lineWidth": 3
}
```

## Arrow

Uses `x1, y1` (start) and `x2, y2` (end).

```json
{
  "type": "arrow",
  "x1": 100,
  "y1": 100,
  "x2": 300,
  "y2": 100,
  "color": "#2c3e50",
  "lineWidth": 3,
  "arrowType": "single",
  "arrowHeadStyle": "triangle",
  "arrowHeadSize": "medium",
  "headSize": 12
}
```

### Arrow Options

| Property | Values | Description |
|----------|--------|-------------|
| `arrowType` | `"single"`, `"double"`, `"none"` | Arrow head placement |
| `arrowHeadStyle` | `"triangle"`, `"open"`, `"diamond"`, `"circle"` | Head style |
| `arrowHeadSize` | `"small"`, `"medium"`, `"large"` | Size multiplier |
| `headSize` | number | Base head size in pixels (default: 12) |

### Curved Arrow

```json
{
  "type": "arrow",
  "x1": 100,
  "y1": 100,
  "x2": 300,
  "y2": 100,
  "controlPoint": { "x": 200, "y": 50 },
  "color": "#2c3e50",
  "lineWidth": 3,
  "arrowType": "single"
}
```

## Line/Arrow Connections

Lines and arrows can connect to shapes. When a connected shape moves, the line endpoint automatically updates to maintain the connection.

### Connection Properties

| Property | Type | Description |
|----------|------|-------------|
| `startConnection` | object | Connection data for start endpoint (x1, y1) |
| `endConnection` | object | Connection data for end endpoint (x2, y2) |

### Connection Types

**Edge Connection** - Endpoint snaps to shape edge:
```json
{
  "shapeId": "rect_123",
  "shapeIndex": 0,
  "edge": "right",
  "t": 0.5
}
```

**Inside Connection** - Endpoint inside shape bounds:
```json
{
  "shapeId": "circle_456",
  "shapeIndex": 2,
  "inside": true,
  "relativeX": 0.5,
  "relativeY": 0.5
}
```

### Edge Names by Shape Type

| Shape | Edge Names |
|-------|------------|
| Rectangle, Image, Text, Frame | `top`, `right`, `bottom`, `left` |
| Circle, Ellipse | `circumference` |
| Diamond | `top-right`, `bottom-right`, `bottom-left`, `top-left` |
| Triangle | `edge-0`, `edge-1`, `edge-2` |
| Path | `segment-N` (N = segment index) |

### Connected Arrow Example

```json
{
  "type": "arrow",
  "x1": 150,
  "y1": 200,
  "x2": 350,
  "y2": 200,
  "color": "#2c3e50",
  "lineWidth": 2,
  "arrowType": "single",
  "startConnection": {
    "shapeId": "rect_1",
    "shapeIndex": 0,
    "edge": "right",
    "t": 0.5
  },
  "endConnection": {
    "shapeId": "circle_2",
    "shapeIndex": 1,
    "edge": "circumference",
    "t": 0.75
  }
}
```

**Connectable Shapes:** All shapes except line, arrow, audio, effect, filter, transition, viewportKeyframe, cursor.

## Text

`x, y` is the **top-left** by default. When `align: "center"`, `x` is the **center point**.

```json
{
  "type": "text",
  "x": 100,
  "y": 100,
  "text": "Hello World",
  "fontSize": 48,
  "fontFamily": "Arial",
  "color": "#333333",
  "bold": true,
  "italic": false,
  "underline": false,
  "align": "left",
  "lineHeight": 1.3
}
```

### Text Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `text` | string | "" | Text content (supports `\n` for newlines) |
| `fontSize` | number | 16 | Font size in pixels |
| `fontFamily` | string | "sans-serif" | Font family |
| `color` | string | "#1e1e1e" | Text color |
| `bold` | boolean | false | Bold text |
| `italic` | boolean | false | Italic text |
| `underline` | boolean | false | Underlined text |
| `align` | string | "left" | `"left"`, `"center"`, `"right"` |
| `lineHeight` | number | 1.3 | Line height multiplier |
| `width` | number | - | If set, enables text wrapping |

### Text Box (Word Wrap)

```json
{
  "type": "text",
  "x": 100,
  "y": 100,
  "width": 300,
  "text": "This is a long text that will automatically wrap within the specified width.",
  "fontSize": 16,
  "fontFamily": "Arial",
  "color": "#333333",
  "align": "left"
}
```

## Image

```json
{
  "type": "image",
  "x": 100,
  "y": 100,
  "width": 400,
  "height": 300,
  "src": "https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=800",
  "opacity": 100,
  "cornerRadius": 10
}
```

### Image with Crop

```json
{
  "type": "image",
  "x": 100,
  "y": 100,
  "width": 400,
  "height": 300,
  "src": "https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=800",
  "cropX": 100,
  "cropY": 50,
  "cropWidth": 600,
  "cropHeight": 400
}
```

**CORS-friendly image sources**:
- Unsplash: `https://images.unsplash.com/photo-[id]`
- Picsum: `https://picsum.photos/[width]/[height]`
- Placeholder: `https://via.placeholder.com/[size]`

## Path (Custom Shapes)

Paths allow creating custom shapes using bezier curves. Segments use `point: [x, y]` array format.

```json
{
  "type": "path",
  "x": 0,
  "y": 0,
  "segments": [
    { "point": [0, 50] },
    { "point": [50, 0], "handleIn": [-15, 0], "handleOut": [15, 0] },
    { "point": [100, 50], "handleIn": [0, -30] }
  ],
  "closed": true,
  "fillColor": "#e74c3c"
}
```

### Segment Properties

| Property | Type | Description |
|----------|------|-------------|
| `point` | `[x, y]` | Anchor point position (array format) |
| `handleIn` | `[dx, dy]` | Incoming bezier handle (relative offset) |
| `handleOut` | `[dx, dy]` | Outgoing bezier handle (relative offset) |

### Common Path Shapes

#### Heart
```json
{
  "type": "path",
  "segments": [
    { "point": [50, 80] },
    { "point": [0, 30], "handleIn": [20, 30], "handleOut": [-15, -20] },
    { "point": [50, 0], "handleIn": [-25, 0], "handleOut": [25, 0] },
    { "point": [100, 30], "handleIn": [15, -20], "handleOut": [-20, 30] }
  ],
  "closed": true,
  "fillColor": "#e74c3c"
}
```

#### Star (5-pointed)
```json
{
  "type": "path",
  "segments": [
    { "point": [50, 0] },
    { "point": [61, 35] },
    { "point": [100, 38] },
    { "point": [68, 60] },
    { "point": [79, 100] },
    { "point": [50, 75] },
    { "point": [21, 100] },
    { "point": [32, 60] },
    { "point": [0, 38] },
    { "point": [39, 35] }
  ],
  "closed": true,
  "fillColor": "#f1c40f"
}
```

#### Hexagon
```json
{
  "type": "path",
  "segments": [
    { "point": [50, 0] },
    { "point": [100, 25] },
    { "point": [100, 75] },
    { "point": [50, 100] },
    { "point": [0, 75] },
    { "point": [0, 25] }
  ],
  "closed": true,
  "fillColor": "#3498db"
}
```

### Compound Paths (Shapes with Holes)

Compound paths contain multiple child paths. Inner paths create holes using even-odd fill rule.

```json
{
  "type": "path",
  "isCompound": true,
  "fillColor": "#3498db",
  "children": [
    {
      "segments": [
        { "point": [0, 0] },
        { "point": [100, 0] },
        { "point": [100, 100] },
        { "point": [0, 100] }
      ],
      "closed": true
    },
    {
      "segments": [
        { "point": [25, 25] },
        { "point": [75, 25] },
        { "point": [75, 75] },
        { "point": [25, 75] }
      ],
      "closed": true
    }
  ]
}
```

This creates a square with a square hole in the center.

## Group

Groups combine shapes that move/transform together:

```json
{
  "type": "group",
  "x": 100,
  "y": 100,
  "rotation": 45,
  "scaleX": 1,
  "scaleY": 1,
  "opacity": 100,
  "children": [
    { "type": "rect", "x": 0, "y": 0, "width": 50, "height": 50, "fillColor": "#e74c3c" },
    { "type": "circle", "x": 50, "y": 50, "radius": 25, "fillColor": "#3498db" }
  ]
}
```

Children positions are **relative** to the group's x,y.

### Group Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `x`, `y` | number | 0 | Group position offset |
| `rotation` | number | 0 | Rotation in degrees |
| `scaleX` | number | 1 | Horizontal scale |
| `scaleY` | number | 1 | Vertical scale |
| `opacity` | number | 100 | Group opacity (0-100) |
| `children` | array | [] | Child shapes |

## Frame

Frames are containers that group shapes (like Figma frames):

```json
{
  "type": "frame",
  "x": 0,
  "y": 0,
  "width": 400,
  "height": 300,
  "name": "My Frame",
  "fillColor": "#f5f5f5",
  "color": "#6366f1",
  "lineWidth": 1,
  "clipContent": false,
  "cornerRadius": 8,
  "children": [
    { "type": "rect", "x": 20, "y": 20, "width": 100, "height": 100, "fillColor": "#3498db" },
    { "type": "text", "x": 20, "y": 140, "text": "Inside frame", "fontSize": 16, "color": "#333" }
  ]
}
```

**Important**: Frames cannot rotate (rotation is ignored). Children positions are relative to the frame.

### Frame Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `name` | string | "Frame" | Frame label (shown above frame) |
| `clipContent` | boolean | true | When true, children outside frame bounds are hidden |
| `cornerRadius` | number | 0 | Corner radius |
| `cornerRadii` | object | - | Individual corners `{ tl, tr, br, bl }` |

### clipContent Behavior

- `clipContent: true` (default) - Children that extend beyond the frame boundaries are clipped/hidden
- `clipContent: false` - Children can overflow and render outside the frame bounds

## Freehand Drawing

```json
{
  "type": "freehand",
  "color": "#1e1e1e",
  "lineWidth": 2,
  "points": [
    { "x": 100, "y": 100 },
    { "x": 105, "y": 102 },
    { "x": 110, "y": 105 },
    { "x": 120, "y": 108 }
  ]
}
```

## 3D Tilt

Shapes can be tilted in 3D space:

| Property | Range | Description |
|----------|-------|-------------|
| `tiltX` | -1.047 to 1.047 | Forward/backward tilt (radians) |
| `tiltY` | -1.047 to 1.047 | Left/right tilt (radians) |

Common values:
- 15° = 0.26 radians (subtle)
- 30° = 0.52 radians (moderate)
- 45° = 0.79 radians (strong)
- 60° = 1.05 radians (maximum)

```json
{
  "type": "rect",
  "x": 100,
  "y": 100,
  "width": 200,
  "height": 150,
  "fillColor": "#3498db",
  "tiltX": 0.5,
  "tiltY": 0.3
}
```