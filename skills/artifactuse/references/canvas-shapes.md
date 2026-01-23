# Canvas Shapes Reference

Complete reference for all shape types in Artifactuse.

## Common Properties (All Shapes)

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `x` | number | 0 | X position (pixels) |
| `y` | number | 0 | Y position (pixels) |
| `rotation` | number | 0 | Rotation (degrees) |
| `opacity` | number | 1 | Opacity (0-1) |
| `fill` | string | "#000000" | Fill color (hex or "transparent") |
| `stroke` | string | "transparent" | Stroke color |
| `strokeWidth` | number | 1 | Stroke width (pixels) |
| `tiltX` | number | 0 | 3D tilt forward/backward in radians (±1.047 max) |
| `tiltY` | number | 0 | 3D tilt left/right in radians (±1.047 max) |

## Rectangle

```json
{
  "type": "rect",
  "x": 100,
  "y": 100,
  "width": 200,
  "height": 150,
  "fill": "#3498db",
  "stroke": "#2980b9",
  "strokeWidth": 2,
  "cornerRadius": 10
}
```

## Circle

```json
{
  "type": "circle",
  "x": 200,
  "y": 200,
  "radius": 50,
  "fill": "#e74c3c",
  "stroke": "#c0392b",
  "strokeWidth": 2
}
```

## Ellipse

```json
{
  "type": "ellipse",
  "x": 200,
  "y": 200,
  "radiusX": 80,
  "radiusY": 50,
  "fill": "#9b59b6"
}
```

## Diamond

```json
{
  "type": "diamond",
  "x": 100,
  "y": 100,
  "width": 100,
  "height": 100,
  "fill": "#f39c12"
}
```

## Triangle

```json
{
  "type": "triangle",
  "x": 100,
  "y": 100,
  "width": 100,
  "height": 100,
  "fill": "#1abc9c"
}
```

## Line

```json
{
  "type": "line",
  "x": 100,
  "y": 100,
  "x2": 300,
  "y2": 200,
  "stroke": "#34495e",
  "strokeWidth": 3
}
```

## Arrow

```json
{
  "type": "arrow",
  "x": 100,
  "y": 100,
  "x2": 300,
  "y2": 100,
  "stroke": "#2c3e50",
  "strokeWidth": 3,
  "arrowType": "single",
  "arrowHeadStyle": "triangle",
  "arrowSize": "medium"
}
```

Arrow options:
- `arrowType`: "single", "double", "none"
- `arrowHeadStyle`: "triangle", "open", "diamond", "circle"
- `arrowSize`: "small", "medium", "large"

## Text

```json
{
  "type": "text",
  "x": 100,
  "y": 100,
  "text": "Hello World",
  "fontSize": 48,
  "fontFamily": "Arial",
  "fill": "#333333",
  "fontWeight": "bold",
  "fontStyle": "normal",
  "textAlign": "center",
  "lineHeight": 1.2
}
```

**Note**: When `textAlign: "center"`, the `x` position is the center point of the text.

## Image

```json
{
  "type": "image",
  "x": 100,
  "y": 100,
  "width": 400,
  "height": 300,
  "src": "https://images.unsplash.com/photo-1506905925346-21bda4d32df4?w=800",
  "opacity": 1,
  "cropX": 0,
  "cropY": 0,
  "cropWidth": 400,
  "cropHeight": 300
}
```

**CORS-friendly image sources**:
- Unsplash: `https://images.unsplash.com/photo-[id]`
- Picsum: `https://picsum.photos/[width]/[height]`
- Placeholder: `https://via.placeholder.com/[size]`

## Path (Custom Shapes)

Paths allow creating custom shapes using bezier curves:

```json
{
  "type": "path",
  "x": 100,
  "y": 100,
  "segments": [
    { "x": 0, "y": 50 },
    { "x": 50, "y": 0, "handleIn": { "x": 0, "y": -30 }, "handleOut": { "x": 30, "y": 0 } },
    { "x": 100, "y": 50, "handleIn": { "x": 0, "y": -30 } }
  ],
  "closed": true,
  "fill": "#e74c3c"
}
```

### Path Segment Properties

| Property | Type | Description |
|----------|------|-------------|
| `x`, `y` | number | Anchor point position (relative to shape x,y) |
| `handleIn` | object | Incoming bezier handle `{ x, y }` |
| `handleOut` | object | Outgoing bezier handle `{ x, y }` |

### Common Path Shapes

#### Heart
```json
{
  "type": "path",
  "x": 100,
  "y": 100,
  "segments": [
    { "x": 50, "y": 80 },
    { "x": 0, "y": 30, "handleIn": { "x": 20, "y": 30 }, "handleOut": { "x": -15, "y": -20 } },
    { "x": 50, "y": 0, "handleIn": { "x": -25, "y": 0 }, "handleOut": { "x": 25, "y": 0 } },
    { "x": 100, "y": 30, "handleIn": { "x": 15, "y": -20 }, "handleOut": { "x": -20, "y": 30 } }
  ],
  "closed": true,
  "fill": "#e74c3c"
}
```

#### Star (5-pointed)
```json
{
  "type": "path",
  "x": 100,
  "y": 100,
  "segments": [
    { "x": 50, "y": 0 },
    { "x": 61, "y": 35 },
    { "x": 100, "y": 38 },
    { "x": 68, "y": 60 },
    { "x": 79, "y": 100 },
    { "x": 50, "y": 75 },
    { "x": 21, "y": 100 },
    { "x": 32, "y": 60 },
    { "x": 0, "y": 38 },
    { "x": 39, "y": 35 }
  ],
  "closed": true,
  "fill": "#f1c40f"
}
```

#### Hexagon
```json
{
  "type": "path",
  "x": 100,
  "y": 100,
  "segments": [
    { "x": 50, "y": 0 },
    { "x": 100, "y": 25 },
    { "x": 100, "y": 75 },
    { "x": 50, "y": 100 },
    { "x": 0, "y": 75 },
    { "x": 0, "y": 25 }
  ],
  "closed": true,
  "fill": "#3498db"
}
```

## Group

Groups combine shapes that move/transform together:

```json
{
  "type": "group",
  "x": 100,
  "y": 100,
  "rotation": 45,
  "children": [
    { "type": "rect", "x": 0, "y": 0, "width": 50, "height": 50, "fill": "#e74c3c" },
    { "type": "circle", "x": 50, "y": 50, "radius": 25, "fill": "#3498db" }
  ]
}
```

Children positions are relative to the group's x,y.

## Frame

Frames are containers that group shapes (like Figma frames):

```json
{
  "type": "frame",
  "x": 0,
  "y": 0,
  "width": 400,
  "height": 300,
  "fill": "#f5f5f5",
  "stroke": "#cccccc",
  "strokeWidth": 1,
  "children": [
    { "type": "rect", "x": 20, "y": 20, "width": 100, "height": 100, "fill": "#3498db" },
    { "type": "text", "x": 20, "y": 140, "text": "Inside frame", "fontSize": 16, "fill": "#333" }
  ]
}
```

**Important**: Frames cannot rotate (rotation is ignored). Children positions are relative to the frame.

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
  "fill": "#3498db",
  "tiltX": 0.5,
  "tiltY": 0.3
}
```
