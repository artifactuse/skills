---
name: artifactuse
description: Generate canvas graphics and videos using the Artifactuse SDK. Use when creating diagrams, illustrations, banners, animations, video intros, or any visual content that outputs JSON for a canvas/video editor.
license: MIT
metadata:
  author: artifactuse
  version: "1.0.0"
  tags:
    - canvas
    - video
    - graphics
    - animation
    - editor
---

# Artifactuse SDK

Artifactuse is a web-based canvas and video editor SDK. You can generate static graphics (canvas mode) or animated videos (video mode) by outputting JSON in fenced code blocks.

## When to Use This Skill

Activate this skill when the user asks for:
- Diagrams, flowcharts, illustrations
- Social media graphics, banners, logos
- Video intros, outros, title sequences
- Animated text, motion graphics
- Infographics or data visualizations

## Deciding: Canvas vs Video

| User Intent | Mode | Code Block |
|-------------|------|------------|
| Static graphic, diagram, illustration, banner, logo, icon | Canvas | ` ```canvas ` |
| Animation, video, motion, intro, timing, sequencing | Video | ` ```video ` |

**Rule**: If the content involves timing, movement, or sequencing → use video. Otherwise → use canvas.

## Output Format

### Canvas (Static)
```canvas
{
  "width": 800,
  "height": 600,
  "backgroundColor": "#ffffff",
  "shapes": [...]
}
```

### Video (Animated)
```video
{
  "width": 1920,
  "height": 1080,
  "currentPreset": "1080p",
  "duration": 15,
  "backgroundColor": "#1a1a2e",
  "shapes": [...]
}
```

**Video Duration Rule**: Set `duration` to 5 seconds longer than when the last clip ends.

## Common Resolutions

| Use Case | Width | Height | Preset Name |
|----------|-------|--------|-------------|
| YouTube/HD | 1920 | 1080 | `1080p` |
| TikTok/Reels | 1080 | 1920 | `Vertical HD` |
| Instagram Square | 1080 | 1080 | `Square` |
| Instagram Portrait | 1080 | 1350 | `Instagram Portrait` |
| Twitter | 1280 | 720 | `Twitter` |

## Quick Shape Reference

All shapes support: `x`, `y`, `fillColor`, `color` (stroke), `lineWidth`, `rotation`, `opacity`

| Type | Key Properties |
|------|----------------|
| `rect` | `width`, `height`, `cornerRadius` |
| `circle` | `radius` (x,y is center) |
| `ellipse` | `radiusX`, `radiusY` (x,y is center) |
| `text` | `text`, `fontSize`, `fontFamily`, `bold`, `italic`, `align` |
| `image` | `src`, `width`, `height` |
| `line` | `x1`, `y1`, `x2`, `y2` |
| `arrow` | `x1`, `y1`, `x2`, `y2`, `arrowType`, `arrowHeadStyle` |
| `path` | `segments[]`, `closed` |
| `group` | `children[]` |
| `frame` | `children[]` (no rotation) |
| `diamond` | `width`, `height` (x,y is center) |
| `triangle` | `size` (x,y is center) |

**For video mode, shapes also support**: `startTime`, `duration`, `trackId`

## References

For detailed information, read these reference files:

- **[references/canvas-shapes.md](./references/canvas-shapes.md)** — Complete shape properties and examples
- **[references/video-timeline.md](./references/video-timeline.md)** — Temporal properties, tracks, audio, video clips
- **[references/animations.md](./references/animations.md)** — The `fx` array animation system, easing, effects

## Critical Rules

1. **Frame boundaries**: All elements must stay within canvas dimensions
2. **Video tracks**: Overlapping clips MUST have different `trackId` values
3. **Animations**: Use the `fx` array on shapes, NOT a `keyframes` property
4. **Media sources**: Use CORS-friendly URLs (Unsplash, Pexels, Pixabay)
5. **Centered text**: When `align: "center"`, the `x` is the center point
6. **Property names**: Use `fillColor` for fill, `color` for stroke, `lineWidth` for stroke width

## Quick Example: Canvas

```canvas
{
  "width": 800,
  "height": 600,
  "backgroundColor": "#ffffff",
  "shapes": [
    {
      "type": "circle",
      "x": 400,
      "y": 300,
      "radius": 100,
      "fillColor": "#3498db",
      "color": "#2980b9",
      "lineWidth": 3
    }
  ]
}
```

## Quick Example: Animated Title

```video
{
  "width": 1920,
  "height": 1080,
  "currentPreset": "1080p",
  "duration": 10,
  "backgroundColor": "#1a1a2e",
  "shapes": [
    {
      "type": "text",
      "x": 960,
      "y": 540,
      "text": "Welcome",
      "fontSize": 120,
      "fontFamily": "Arial",
      "bold": true,
      "color": "#ffffff",
      "align": "center",
      "startTime": 0,
      "duration": 5,
      "fx": [
        { "type": "animation", "name": "fadeIn", "duration": 0.5, "position": "in" },
        { "type": "animation", "name": "zoomIn", "duration": 0.5, "position": "in" }
      ]
    }
  ]
}
```

## Color Palettes

**Dark Theme**: Background `#1a1a2e`, Accent `#4a90d9`, Text `#ffffff`
**Light Theme**: Background `#ffffff`, Accent `#3498db`, Text `#2c3e50`