# Animations Reference

Artifactuse supports animations through the `fx` array on individual shapes.

## Important Limitations

**NOT Supported:**
- ❌ `keyframes` property for per-property animation
- ❌ `scale` property on shapes (use width/height or fontSize instead)
- ❌ Animating arbitrary properties over time
- ❌ CSS-style keyframe animations
- ❌ Nested text inside shapes

**Use Instead:**
- ✅ The `fx` array for entrance/exit animations
- ✅ Timeline effects for global filters
- ✅ Viewport keyframes for camera animation

## The `fx` Array

Add an `fx` array to any shape to apply filters and animations:

```json
{
  "type": "rect",
  "x": 100,
  "y": 100,
  "width": 400,
  "height": 300,
  "fill": "#3498db",
  "startTime": 0,
  "duration": 5,
  "fx": [
    {
      "type": "filter",
      "name": "brightness",
      "value": 120
    },
    {
      "type": "animation",
      "name": "slideUp",
      "duration": 0.5,
      "position": "in",
      "easing": "ease-out"
    },
    {
      "type": "animation",
      "name": "fadeOut",
      "duration": 0.5,
      "position": "out",
      "easing": "ease-in"
    }
  ]
}
```

## Filter FX

```json
{
  "type": "filter",
  "name": "brightness",
  "value": 120
}
```

| Name | Range | Default | Description |
|------|-------|---------|-------------|
| `brightness` | 0-200 | 100 | Brightness (%) |
| `contrast` | 0-200 | 100 | Contrast (%) |
| `saturation` | 0-200 | 100 | Saturation (%) |
| `grayscale` | 0-100 | 0 | Grayscale (%) |
| `sepia` | 0-100 | 0 | Sepia tone (%) |
| `blur` | 0-20 | 0 | Blur (px) |
| `hueRotate` | 0-360 | 0 | Hue rotation (°) |
| `invert` | 0-100 | 0 | Inversion (%) |
| `temperature` | -100 to 100 | 0 | Color temperature |

## Animation FX

```json
{
  "type": "animation",
  "name": "slideUp",
  "duration": 0.5,
  "position": "in",
  "easing": "ease-out"
}
```

| Property | Type | Description |
|----------|------|-------------|
| `name` | string | Animation type |
| `duration` | number | Duration in seconds (default: 0.5) |
| `position` | string | "in" (entrance), "out" (exit), or "both" |
| `easing` | string | "linear", "ease-in", "ease-out", "ease-in-out" |

**Limitation**: Each shape supports one animation per position.

### Available Animations

| Animation | Description |
|-----------|-------------|
| `fade` | Opacity fade |
| `slideLeft` | Slide from left |
| `slideRight` | Slide from right |
| `slideUp` | Slide from bottom |
| `slideDown` | Slide from top |
| `zoom` | Scale animation |
| `zoomIn` | Scale up |
| `zoomOut` | Scale down |
| `crossZoom` | Cross zoom |
| `wipe` | Wipe effect |
| `blur` | Blur in/out |
| `dissolve` | Dissolve effect |
| `spin` | Rotation (Z-axis) |
| `flip` | Flip (Y-axis) |
| `bounce` | Bouncy effect |
| `elastic` | Elastic spring |
| `rotate3d` | 3D rotation |

### 3D Animation Example

```json
{
  "type": "rect",
  "x": 760,
  "y": 440,
  "width": 400,
  "height": 200,
  "fill": "#3498db",
  "startTime": 0,
  "duration": 5,
  "fx": [
    {
      "type": "animation",
      "name": "rotate3d",
      "duration": 0.8,
      "position": "in",
      "easing": "ease-out"
    },
    {
      "type": "animation",
      "name": "rotate3d",
      "duration": 0.6,
      "position": "out",
      "easing": "ease-in"
    }
  ]
}
```

## Cursor Animation

Animated cursors for tutorials and demos:

```json
{
  "type": "cursor",
  "cursorType": "pointer",
  "fillColor": "#ffffff",
  "color": "#000000",
  "cursorScale": 1.0,
  "startTime": 0,
  "duration": 5,
  "trackId": "track-2",
  "cursorKeyframes": [
    { "time": 0, "x": 500, "y": 300, "easing": "ease-out" },
    { "time": 1.5, "x": 800, "y": 450, "holdTime": 0.5, "easing": "ease-in-out" },
    { "time": 1, "x": 1200, "y": 400 }
  ],
  "clicks": [
    { "time": 2, "effect": "ripple", "color": "#4a90d9", "size": 40, "duration": 0.4 }
  ],
  "showPath": false
}
```

**Important**: `cursorKeyframes[].time` is the **travel duration** to that keyframe, not absolute time.

### Cursor Types

| Type | Description |
|------|-------------|
| `pointer` | Standard arrow (default) |
| `hand` | Open hand for clickable |
| `crosshair` | Precision targeting |
| `grab` | Open hand for draggable |
| `grabbing` | Closed hand during drag |

### Keyframe Properties

| Property | Type | Description |
|----------|------|-------------|
| `time` | number | Travel duration (seconds) |
| `x`, `y` | number | Position |
| `holdTime` | number | Pause at position (seconds) |
| `easing` | string | Easing to next keyframe |
| `controlIn` | object | Bezier control `{ x, y }` |
| `controlOut` | object | Bezier control `{ x, y }` |

### Click Effects

| Effect | Description |
|--------|-------------|
| `ripple` | Expanding circle (Material style) |
| `highlight` | Solid circle that shrinks |
| `pulse` | Multiple expanding rings |

### Click Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `time` | number | - | When click occurs |
| `effect` | string | "ripple" | Effect type |
| `color` | string | "#4a90d9" | Effect color |
| `size` | number | 40 | Effect size (px) |
| `duration` | number | 0.4 | Effect duration |

## Camera Animation (Viewport Keyframes)

Create pan and zoom effects:

```json
{
  "type": "viewportKeyframe",
  "startTime": 3,
  "trackId": "track-camera",
  "viewport": {
    "x": -400,
    "y": -200,
    "zoom": 2.0
  },
  "easing": "ease-in-out"
}
```

| Property | Type | Description |
|----------|------|-------------|
| `viewport.x` | number | Pan X offset |
| `viewport.y` | number | Pan Y offset |
| `viewport.zoom` | number | Zoom level (1.0 = 100%) |
| `easing` | string | Transition easing |

### Camera Animation Example

```json
{
  "shapes": [
    {
      "type": "viewportKeyframe",
      "startTime": 0,
      "viewport": { "x": 0, "y": 0, "zoom": 1.0 },
      "easing": "ease-out"
    },
    {
      "type": "viewportKeyframe",
      "startTime": 3,
      "viewport": { "x": -400, "y": -200, "zoom": 2.0 },
      "easing": "ease-in-out"
    },
    {
      "type": "viewportKeyframe",
      "startTime": 6,
      "viewport": { "x": 0, "y": 0, "zoom": 1.0 },
      "easing": "ease-in"
    }
  ]
}
```

This zooms into a specific area at 3 seconds, then zooms back out at 6 seconds.

## Timeline Effects

Global effects placed on FX tracks:

```json
{
  "type": "effect",
  "subtype": "dropShadow",
  "name": "Drop Shadow",
  "startTime": 0,
  "duration": 5,
  "trackId": "track-fx-1",
  "intensity": 100,
  "offsetX": 5,
  "offsetY": 5,
  "blur": 10,
  "color": "rgba(0,0,0,0.5)"
}
```

| Subtype | Parameters | Description |
|---------|-----------|-------------|
| `dropShadow` | offsetX, offsetY, blur, color | Drop shadow |
| `glow` | radius, color | Outer glow |
| `outline` | width, color | Stroke outline |
| `vignette` | radius | Dark edges |
| `blur` | radius | Gaussian blur |
| `grain` | amount | Film grain |
| `glitch` | intensity, frequency | Digital glitch |
| `chromatic` | offset | Chromatic aberration |
| `pixelate` | size | Pixelation |
| `sharpen` | amount | Sharpening |
| `emboss` | - | Embossed 3D |
