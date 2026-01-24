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

Available subtypes:
- `fade`, `dissolve`
- `wipeLeft`, `wipeRight`, `wipeUp`, `wipeDown`
- `slideLeft`, `slideRight`, `slideUp`, `slideDown`
- `zoomIn`, `zoomOut`, `crossZoom`
- `blur`, `spin`, `flip`, `bounce`, `elastic`

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