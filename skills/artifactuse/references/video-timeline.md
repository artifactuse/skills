# Video Timeline Reference

Video mode extends canvas with temporal properties, tracks, audio, and video clips.

## Temporal Properties

Every shape in video mode can have these properties:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `startTime` | number | 0 | When shape appears (seconds) |
| `duration` | number | project duration | How long shape is visible |
| `trackId` | string | auto | Track assignment |
| `mediaStartOffset` | number | 0 | For trimmed clips, offset into source media |

## Project Duration Rule

**IMPORTANT**: Always set project `duration` to 5 seconds longer than when the last clip ends.

Example: If last shape ends at `startTime: 8` + `duration: 4` = 12 seconds → set project `duration` to 17.

## Tracks

Tracks organize shapes vertically in the timeline:
- Higher tracks render on top (z-index)
- Track IDs: `track-1`, `track-2`, `track-3`, etc.
- FX tracks: `track-fx-1`, `track-fx-2`, etc.

### Track Assignment Rules

| Scenario | trackId Required? | Recommendation |
|----------|-------------------|----------------|
| Sequential clips (no overlap) | Optional | Can omit `trackId` |
| Overlapping clips | **Required** | Must specify different `trackId` values |
| Background + foreground | **Required** | Background on `track-1`, elements on higher tracks |
| Audio | Recommended | Use `track-audio` |

### FX Tracks

FX tracks are special tracks for effects, filters, and transitions. They differ from regular video/audio tracks:

| Track Type | ID Format | Purpose |
|------------|-----------|---------|
| Video tracks | `track-1`, `track-2`, etc. | Hold video, image, text, shapes |
| Audio tracks | `track-audio`, `audio-1`, etc. | Hold audio clips |
| FX tracks | `track-fx-1`, `track-fx-2`, etc. | Hold effects, filters, transitions |

**FX Track Behavior:**
- Created automatically when you add the first effect/filter/transition
- Multiple FX tracks can exist (for overlapping effects)
- FX clips on higher tracks are applied later (stacking order)
- Effects on FX tracks apply to the **entire canvas** during their time range

```json
{
  "type": "effect",
  "subtype": "vignette",
  "startTime": 0,
  "duration": 10,
  "trackId": "track-fx-1",
  "intensity": 80,
  "radius": 50
}
```

### Example: Overlapping Clips (trackId required)

```json
{
  "width": 1920,
  "height": 1080,
  "duration": 15,
  "backgroundColor": "#1a1a2e",
  "shapes": [
    {
      "type": "rect",
      "trackId": "track-1",
      "startTime": 0,
      "duration": 10,
      "x": 0,
      "y": 0,
      "width": 1920,
      "height": 1080,
      "fillColor": "#1a1a2e"
    },
    {
      "type": "text",
      "trackId": "track-2",
      "startTime": 2,
      "duration": 5,
      "x": 960,
      "y": 540,
      "text": "Title",
      "fontSize": 72,
      "color": "#ffffff",
      "align": "center"
    },
    {
      "type": "image",
      "trackId": "track-3",
      "startTime": 3,
      "duration": 4,
      "x": 100,
      "y": 100,
      "width": 200,
      "height": 150,
      "src": "https://picsum.photos/200/150"
    }
  ]
}
```

### Example: Sequential Clips (trackId optional)

```json
{
  "shapes": [
    { "type": "text", "startTime": 0, "duration": 2, "x": 960, "y": 540, "text": "First", "fontSize": 48, "color": "#fff", "align": "center" },
    { "type": "text", "startTime": 2, "duration": 2, "x": 960, "y": 540, "text": "Second", "fontSize": 48, "color": "#fff", "align": "center" },
    { "type": "text", "startTime": 4, "duration": 2, "x": 960, "y": 540, "text": "Third", "fontSize": 48, "color": "#fff", "align": "center" }
  ]
}
```

## Video Clips

```json
{
  "type": "video",
  "x": 0,
  "y": 0,
  "width": 1920,
  "height": 1080,
  "src": "https://videos.pexels.com/video-files/3015488/3015488-hd_1920_1080_24fps.mp4",
  "startTime": 0,
  "duration": 10,
  "mediaStartOffset": 5,
  "volume": 100,
  "fadeIn": 0.5,
  "fadeOut": 0.5,
  "cornerRadius": 0,
  "strokeColor": "transparent",
  "lineWidth": 2
}
```

### Video Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `src` | string | - | Video URL (must be CORS-friendly) |
| `volume` | number | 100 | Volume level (0-100) |
| `fadeIn` | number | 0 | Audio fade in duration (seconds) |
| `fadeOut` | number | 0 | Audio fade out duration (seconds) |
| `mediaStartOffset` | number | 0 | Start playback from this offset |
| `cornerRadius` | number | 0 | Rounded corners |
| `strokeColor` | string | - | Border color |
| `lineWidth` | number | 1 | Border width |
| `cropX`, `cropY` | number | - | Crop offset in source |
| `cropWidth`, `cropHeight` | number | - | Crop dimensions |

**CORS-friendly video sources**:
- Pexels: `https://videos.pexels.com/video-files/[id]/[filename].mp4`
- Pixabay: `https://cdn.pixabay.com/video/[year]/[month]/[day]/[id].mp4`
- Coverr: `https://storage.coverr.co/videos/[id]`

## Audio Clips

```json
{
  "type": "audio",
  "src": "https://cdn.pixabay.com/audio/2024/11/04/audio-123456.mp3",
  "startTime": 0,
  "duration": 30,
  "volume": 80,
  "fadeIn": 1,
  "fadeOut": 2,
  "muted": false,
  "trackId": "track-audio"
}
```

### Audio Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `src` | string | - | Audio URL |
| `volume` | number | 100 | Volume level (0-100) |
| `fadeIn` | number | 0 | Fade in duration (seconds) |
| `fadeOut` | number | 0 | Fade out duration (seconds) |
| `muted` | boolean | false | Mute audio |

**CORS-friendly audio sources**:
- Pixabay Music: `https://cdn.pixabay.com/audio/[year]/[month]/[day]/audio-[id].mp3`

## Screen Capture

For recording screen shares in video mode:

```json
{
  "type": "screenCapture",
  "x": 0,
  "y": 0,
  "width": 1920,
  "height": 1080,
  "cornerRadius": 0,
  "opacity": 100,
  "startTime": 0,
  "duration": 60,
  "trackId": "track-1"
}
```

## Webcam Capture

Viewport-fixed webcam overlay (stays in corner regardless of pan/zoom):

```json
{
  "type": "webcamCapture",
  "viewportMarginRight": 24,
  "viewportMarginBottom": 24,
  "width": 320,
  "height": 240,
  "cornerRadius": 16,
  "rotation": 0,
  "opacity": 100,
  "strokeColor": null,
  "lineWidth": 2,
  "startTime": 0,
  "duration": 60,
  "trackId": "track-2"
}
```

### Webcam Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `viewportMarginRight` | number | 24 | Distance from right edge (pixels) |
| `viewportMarginBottom` | number | 24 | Distance from bottom edge (pixels) |
| `cornerRadius` | number | 16 | Rounded corners |
| `strokeColor` | string | null | Border color |

## Video Resolutions

| Preset Name | Resolution | Aspect | Use Case |
|-------------|-----------|--------|----------|
| `1080p` | 1920×1080 | 16:9 | YouTube, standard HD |
| `720p` | 1280×720 | 16:9 | Web, smaller files |
| `4K` | 3840×2160 | 16:9 | High quality |
| `Vertical HD` | 1080×1920 | 9:16 | TikTok, Reels, Stories |
| `Square` | 1080×1080 | 1:1 | Instagram, social |
| `Instagram Portrait` | 1080×1350 | 4:5 | Instagram feed |
| `Cinematic` | 2560×1080 | 21:9 | Ultrawide |
| `Twitter` | 1280×720 | 16:9 | Twitter/X |
| `Facebook Cover` | 820×312 | Custom | Facebook cover video |

**Important**: Set BOTH `width`/`height` AND `currentPreset`:

```json
{
  "width": 1080,
  "height": 1920,
  "currentPreset": "Vertical HD",
  "duration": 15,
  "backgroundColor": "#000000",
  "shapes": []
}
```

## Frame Boundaries

All elements must stay within canvas dimensions.

### Safe Areas for 1920×1080

| Element Type | Safe X Range | Safe Y Range |
|--------------|--------------|--------------|
| Full-width background | 0 | 0 |
| Centered text | 960 (center) | 50-1030 |
| Left-aligned text | 50-1870 | 50-1030 |
| Lower third | 50 | 850-1030 |
| Title card | 100-1820 | 100-980 |

### Safe Areas for Vertical (1080×1920)

- **Top safe area**: y ≥ 150 (avoid status bar)
- **Bottom safe area**: y ≤ 1750 (avoid nav bar)
- **Side margins**: x ≥ 50, x + width ≤ 1030

## Timeline Transitions

Transitions are standalone clips on FX tracks that create visual effects between scenes or over specific time ranges.

### Transition Structure

```json
{
  "type": "transition",
  "subtype": "fade",
  "name": "Fade",
  "startTime": 5,
  "duration": 1,
  "trackId": "track-fx-1",
  "easing": "ease-in-out"
}
```

### Transition Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `type` | string | - | Must be `"transition"` |
| `subtype` | string | - | Transition type (see table below) |
| `name` | string | - | Display name |
| `startTime` | number | 0 | When transition starts (seconds) |
| `duration` | number | 1 | Transition duration (seconds) |
| `trackId` | string | - | FX track ID (`track-fx-1`, etc.) |
| `easing` | string | `"linear"` | Easing function |

### Easing Options

| Easing | Description |
|--------|-------------|
| `linear` | Constant speed |
| `ease-in` | Slow start, fast end |
| `ease-out` | Fast start, slow end |
| `ease-in-out` | Slow start and end |

### Available Transitions (19 types)

| Subtype | Name | Description |
|---------|------|-------------|
| `fade` | Fade | Opacity crossfade |
| `dissolve` | Dissolve | Gradual dissolve effect |
| `wipeLeft` | Wipe Left | Wipe from right to left |
| `wipeRight` | Wipe Right | Wipe from left to right |
| `wipeUp` | Wipe Up | Wipe from bottom to top |
| `wipeDown` | Wipe Down | Wipe from top to bottom |
| `slideLeft` | Slide Left | Slide content to the left |
| `slideRight` | Slide Right | Slide content to the right |
| `slideUp` | Slide Up | Slide content upward |
| `slideDown` | Slide Down | Slide content downward |
| `zoomIn` | Zoom In | Zoom into the frame |
| `zoomOut` | Zoom Out | Zoom out of the frame |
| `crossZoom` | Cross Zoom | Zoom with crossfade |
| `kenBurns` | Ken Burns | Pan and zoom effect |
| `blur` | Blur | Blur in/out transition |
| `spin` | Spin | Rotation transition |
| `flip` | Flip | 3D flip effect |
| `bounce` | Bounce | Bouncy transition |
| `elastic` | Elastic | Elastic spring effect |

### Transition Example

```json
{
  "width": 1920,
  "height": 1080,
  "duration": 10,
  "shapes": [
    {
      "type": "text",
      "x": 960,
      "y": 540,
      "text": "Scene 1",
      "fontSize": 72,
      "color": "#ffffff",
      "align": "center",
      "startTime": 0,
      "duration": 5,
      "trackId": "track-1"
    },
    {
      "type": "text",
      "x": 960,
      "y": 540,
      "text": "Scene 2",
      "fontSize": 72,
      "color": "#ffffff",
      "align": "center",
      "startTime": 4,
      "duration": 6,
      "trackId": "track-2"
    },
    {
      "type": "transition",
      "subtype": "wipeLeft",
      "name": "Wipe Left",
      "startTime": 4,
      "duration": 1.5,
      "trackId": "track-fx-1",
      "easing": "ease-in-out"
    }
  ]
}
```

## Timeline Filters

```json
{
  "type": "filter",
  "subtype": "saturation",
  "name": "Saturation",
  "startTime": 0,
  "duration": 5,
  "trackId": "track-fx-2",
  "value": 150,
  "min": 0,
  "max": 200
}
```

| Subtype | Range | Default | Description |
|---------|-------|---------|-------------|
| `brightness` | 0-200 | 100 | Brightness |
| `contrast` | 0-200 | 100 | Contrast |
| `saturation` | 0-200 | 100 | Color saturation |
| `hueRotate` | 0-360 | 0 | Hue rotation |
| `grayscale` | 0-100 | 0 | Grayscale |
| `sepia` | 0-100 | 0 | Sepia tone |
| `invert` | 0-100 | 0 | Inversion |
| `temperature` | -100-100 | 0 | Color temperature |

## Timeline Effects

Global visual effects applied to the **entire canvas** during their time range. Placed on FX tracks.

### Effect Structure

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

### Effect Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `type` | string | - | Must be `"effect"` |
| `subtype` | string | - | Effect type (see table below) |
| `name` | string | - | Display name |
| `startTime` | number | 0 | When effect starts (seconds) |
| `duration` | number | 5 | Effect duration (seconds) |
| `trackId` | string | - | FX track ID (`track-fx-1`, etc.) |
| `intensity` | number | 100 | Effect strength (0-100) |

### Available Effects (11 types)

| Subtype | Name | Properties | Description |
|---------|------|------------|-------------|
| `dropShadow` | Drop Shadow | `offsetX`, `offsetY`, `blur`, `color` | Drop shadow effect |
| `glow` | Glow | `radius`, `color` | Outer glow effect |
| `outline` | Outline | `width`, `color` | Stroke outline |
| `vignette` | Vignette | `radius` | Dark edges effect |
| `blur` | Blur | `radius` | Gaussian blur |
| `grain` | Film Grain | `amount` | Film grain noise |
| `glitch` | Glitch | `frequency` | Digital glitch effect |
| `chromatic` | Chromatic Aberration | `offset` | RGB color separation |
| `pixelate` | Pixelate | `size` | Pixelation effect |
| `sharpen` | Sharpen | `amount` | Sharpening |
| `emboss` | Emboss | - | Embossed 3D effect |

### Effect-Specific Properties

**Drop Shadow:**
```json
{
  "type": "effect",
  "subtype": "dropShadow",
  "intensity": 100,
  "offsetX": 5,
  "offsetY": 5,
  "blur": 10,
  "color": "rgba(0,0,0,0.5)"
}
```

**Glow:**
```json
{
  "type": "effect",
  "subtype": "glow",
  "intensity": 100,
  "radius": 10,
  "color": "#ffffff"
}
```

**Vignette:**
```json
{
  "type": "effect",
  "subtype": "vignette",
  "intensity": 80,
  "radius": 50
}
```

**Film Grain:**
```json
{
  "type": "effect",
  "subtype": "grain",
  "intensity": 50,
  "amount": 30
}
```

**Glitch:**
```json
{
  "type": "effect",
  "subtype": "glitch",
  "intensity": 70,
  "frequency": 10
}
```

**Chromatic Aberration:**
```json
{
  "type": "effect",
  "subtype": "chromatic",
  "intensity": 60,
  "offset": 5
}
```

**Pixelate:**
```json
{
  "type": "effect",
  "subtype": "pixelate",
  "intensity": 100,
  "size": 10
}
```

### Effect Example

```json
{
  "width": 1920,
  "height": 1080,
  "duration": 10,
  "shapes": [
    {
      "type": "text",
      "x": 960,
      "y": 540,
      "text": "Cinematic Title",
      "fontSize": 96,
      "color": "#ffffff",
      "align": "center",
      "startTime": 0,
      "duration": 10,
      "trackId": "track-1"
    },
    {
      "type": "effect",
      "subtype": "vignette",
      "name": "Vignette",
      "startTime": 0,
      "duration": 10,
      "trackId": "track-fx-1",
      "intensity": 80,
      "radius": 40
    },
    {
      "type": "effect",
      "subtype": "grain",
      "name": "Film Grain",
      "startTime": 0,
      "duration": 10,
      "trackId": "track-fx-2",
      "intensity": 30,
      "amount": 20
    }
  ]
}
```

## Effect Systems Comparison

Artifactuse has two distinct effect systems. Understanding when to use each is important.

### Shape-Level FX vs Timeline Effects

| Aspect | Shape-Level FX (`shape.fx`) | Timeline Effects/Filters/Transitions |
|--------|---------------------------|-------------------------------------|
| **Storage** | Array on individual clip | Standalone clip in `shapes` |
| **Scope** | Single clip only | Entire canvas |
| **Track** | Same track as clip | Dedicated FX tracks |
| **Duration** | Entire clip duration | Custom start/duration |
| **Limit** | 1 animation, unlimited filters | Unlimited (can stack) |
| **Documented in** | animations.md | video-timeline.md |

### When to Use Each

**Use Shape-Level FX (`shape.fx` array) when:**
- Animating a single clip's entrance/exit (fade in, slide up)
- Applying filters to one specific clip (brighten one image)
- Need animation tied to clip's lifecycle

```json
{
  "type": "text",
  "text": "Hello",
  "fx": [
    { "type": "animation", "name": "fadeIn", "duration": 0.5, "position": "in" },
    { "type": "filter", "name": "brightness", "value": 120 }
  ]
}
```

**Use Timeline Effects when:**
- Applying effects to entire composition (vignette, grain)
- Time-limited effects (glitch at specific moment)
- Stacking multiple global effects
- Scene transitions (wipe, fade between clips)

```json
{
  "type": "effect",
  "subtype": "vignette",
  "startTime": 0,
  "duration": 10,
  "trackId": "track-fx-1"
}
```

### Summary Table

| System | Type | Example Use Case |
|--------|------|------------------|
| `shape.fx` animations | Entrance/exit | Text fades in |
| `shape.fx` filters | Per-clip color | Brighten one image |
| Timeline transitions | Scene changes | Wipe between clips |
| Timeline filters | Global color | Desaturate entire video |
| Timeline effects | Visual FX | Add vignette, grain |

For shape-level FX details, see **[animations.md](./animations.md)**.