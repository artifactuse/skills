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


## Tool Usage: Mixing Singular & Plural

You can freely mix singular and plural tools in the same workflow. Use whichever makes sense for each step:

| Tool | Use For |
|------|---------|
| `crafting_artifact_shape` | Adding 1 shape at a time |
| `crafting_artifact_shapes` | Adding multiple shapes in one call |
| `crafting_artifact_clip` | Adding 1 clip at a time |
| `crafting_artifact_clips` | Adding multiple clips in one call |

### Example Workflow: Fitness Gym Banner (Canvas)
```javascript
// 1. Initialize canvas
crafting_artifact_canvas({ width: 2560, height: 1440, backgroundColor: "#0a0a0a" })

// 2. Add background elements in batch
crafting_artifact_shapes([
  { type: "rect", x: 0, y: 0, width: 2560, height: 1440, fillColor: "#1a1a2e" },
  { type: "circle", x: 200, y: 700, radius: 300, fillColor: "#ff3c00", opacity: 20 },
  { type: "circle", x: 2300, y: 400, radius: 400, fillColor: "#ff3c00", opacity: 15 }
])

// 3. Add main title (single shape for precision)
crafting_artifact_shape({ 
  type: "text", x: 1280, y: 600, text: "ELITE FITNESS", 
  fontSize: 140, fontFamily: "Impact", color: "#ffffff", align: "center" 
})

// 4. Add CTA button group in batch
crafting_artifact_shapes([
  { type: "rect", x: 1080, y: 900, width: 400, height: 80, fillColor: "#ff3c00", cornerRadius: 8 },
  { type: "text", x: 1280, y: 955, text: "JOIN NOW", fontSize: 32, color: "#ffffff", align: "center", bold: true }
])

// 5. Add final accent (single shape)
crafting_artifact_shape({ 
  type: "rect", x: 0, y: 1400, width: 2560, height: 40, fillColor: "#ff3c00" 
})
```

### Example Workflow: YouTube Intro (Video)
```javascript
// 1. Initialize video
crafting_artifact_video({ width: 1920, height: 1080, duration: 10, backgroundColor: "#0a0a0a" })

// 2. Add background elements in batch (appear together)
crafting_artifact_clips([
  { type: "rect", x: 0, y: 0, width: 1920, height: 1080, fillColor: "#1a1a2e", startTime: 0, duration: 10, trackId: 1 },
  { type: "circle", x: 200, y: 540, radius: 200, fillColor: "#ff3c00", opacity: 20, startTime: 0, duration: 10, trackId: 2 },
  { type: "circle", x: 1700, y: 300, radius: 300, fillColor: "#ff3c00", opacity: 15, startTime: 0, duration: 10, trackId: 3 }
])

// 3. Add hero title with animation (single clip for precision)
crafting_artifact_clip({ 
  type: "text", x: 960, y: 500, text: "ELITE FITNESS", 
  fontSize: 120, fontFamily: "Impact", color: "#ffffff", align: "center",
  startTime: 0.5, duration: 4, trackId: 4,
  fx: [{ type: "animation", name: "fadeIn", duration: 0.5, position: "in" }]
})

// 4. Add tagline (single clip, appears after title)
crafting_artifact_clip({ 
  type: "text", x: 960, y: 620, text: "TRANSFORM YOUR BODY", 
  fontSize: 36, fontFamily: "Arial", color: "#ff3c00", align: "center",
  startTime: 1.5, duration: 3, trackId: 5,
  fx: [{ type: "animation", name: "slideUp", duration: 0.4, position: "in" }]
})

// 5. Add end screen elements in batch (appear together at the end)
crafting_artifact_clips([
  { type: "rect", x: 660, y: 750, width: 600, height: 80, fillColor: "#ff3c00", cornerRadius: 8, startTime: 3, duration: 2, trackId: 6 },
  { type: "text", x: 960, y: 805, text: "SUBSCRIBE NOW", fontSize: 32, color: "#ffffff", align: "center", bold: true, startTime: 3, duration: 2, trackId: 7 }
])
```

### When to Use Each

| Scenario | Recommended Tool |
|----------|------------------|
| **Canvas** | |
| Background layers, decorative elements | `crafting_artifact_shapes` (batch) |
| Main headline or hero text | `crafting_artifact_shape` (single) |
| Button with label (rect + text) | `crafting_artifact_shapes` (batch) |
| Adding one element after user feedback | `crafting_artifact_shape` (single) |
| Complete section (header, body, footer) | `crafting_artifact_shapes` (batch) |
| Testing if something works | `crafting_artifact_shape` (single) |
| **Video** | |
| Background/ambient elements (same timing) | `crafting_artifact_clips` (batch) |
| Hero text with specific animation | `crafting_artifact_clip` (single) |
| End screen (CTA button + text) | `crafting_artifact_clips` (batch) |
| Sequenced text (title → subtitle) | `crafting_artifact_clip` (single, each) |
| Multiple elements appearing together | `crafting_artifact_clips` (batch) |
| Fine-tuning animation timing | `crafting_artifact_clip` (single) |

### Key Principle

**Batch related elements, isolate important ones.**

- Group shapes/clips that belong together visually or temporally
- Keep hero elements separate for easier adjustments
- When debugging, switch to singular tools to isolate issues
- For video: batch clips with the same `startTime`, isolate clips with unique animations



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