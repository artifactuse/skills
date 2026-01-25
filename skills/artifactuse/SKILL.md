---
name: artifactuse
description: Generate canvas graphics and videos using the Artifactuse SDK. Use when creating diagrams, illustrations, banners, animations, video intros, or any visual content that outputs JSON for a canvas/video editor.
license: MIT
metadata:
  author: artifactuse
  version: "2.1.0"
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

### Canvas (Static / Infinite Canvas)
```canvas
{
  "backgroundColor": "#ffffff",
  "shapes": [...]
}
```

Canvas mode is an **infinite canvas** - shapes can be placed anywhere without bounds. The `width` and `height` properties are optional metadata used only for export purposes (PNG, SVG). They do not constrain where shapes can be placed.

If you need specific export dimensions:
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

Video mode has a **fixed export frame** - `width` and `height` are required and define the visible area.

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


## Available Tools

### Canvas Tools
| Tool | Description |
|------|-------------|
| `craft_canvas` | Initialize a new infinite canvas (dimensions optional, for export only) |
| `craft_shape` | Add a single shape to the canvas |
| `craft_shapes` | Add multiple shapes at once (batch) |
| `update_shape` | Update properties of an existing shape by ID or index |
| `delete_shape` | Delete a shape from the canvas |
| `list_shapes` | List all shapes with their IDs |

### Video Tools
| Tool | Description |
|------|-------------|
| `craft_video` | Initialize a new video with dimensions and duration |
| `craft_clip` | Add a single clip to the video |
| `craft_clips` | Add multiple clips at once (batch) |
| `update_clip` | Update properties of an existing clip by ID or index |
| `delete_clip` | Delete a clip from the video |
| `list_clips` | List all clips with their IDs |

### Other Tools
| Tool | Description |
|------|-------------|
| `craft_code` | Load a code artifact (React, HTML, etc.) |
| `craft_data` | Load a data artifact (JSON, YAML, etc.) |
| `clear` | Clear the current canvas or video |
| `read_docs` | Read SDK documentation |
| `get_status` | Check connection status |


## Tool Usage: Mixing Singular & Plural

You can freely mix singular and plural tools in the same workflow. Use whichever makes sense for each step:

| Tool | Use For |
|------|---------|
| `craft_shape` | Adding 1 shape at a time |
| `craft_shapes` | Adding multiple shapes in one call |
| `craft_clip` | Adding 1 clip at a time |
| `craft_clips` | Adding multiple clips in one call |

### Example Workflow: Fitness Gym Banner (Canvas)
```javascript
// 1. Initialize canvas (dimensions optional, used for export)
craft_canvas({ width: 2560, height: 1440, backgroundColor: "#0a0a0a" })

// 2. Add background elements in batch
craft_shapes({ shapes: [
  { type: "rect", x: 0, y: 0, width: 2560, height: 1440, fillColor: "#1a1a2e" },
  { type: "circle", x: 200, y: 700, radius: 300, fillColor: "#ff3c00", opacity: 20 },
  { type: "circle", x: 2300, y: 400, radius: 400, fillColor: "#ff3c00", opacity: 15 }
]})

// 3. Add main title (single shape for precision)
craft_shape({ shape: { 
  type: "text", x: 1280, y: 600, text: "ELITE FITNESS", 
  fontSize: 140, fontFamily: "Impact", color: "#ffffff", align: "center" 
}})

// 4. Add CTA button group in batch
craft_shapes({ shapes: [
  { type: "rect", x: 1080, y: 900, width: 400, height: 80, fillColor: "#ff3c00", cornerRadius: 8 },
  { type: "text", x: 1280, y: 955, text: "JOIN NOW", fontSize: 32, color: "#ffffff", align: "center", bold: true }
]})

// 5. Add final accent (single shape)
craft_shape({ shape: { 
  type: "rect", x: 0, y: 1400, width: 2560, height: 40, fillColor: "#ff3c00" 
}})
```

### Example Workflow: YouTube Intro (Video)
```javascript
// 1. Initialize video
craft_video({ width: 1920, height: 1080, duration: 10, backgroundColor: "#0a0a0a" })

// 2. Add background elements in batch (appear together)
craft_clips({ clips: [
  { type: "rect", x: 0, y: 0, width: 1920, height: 1080, fillColor: "#1a1a2e", startTime: 0, duration: 10, trackId: 1 },
  { type: "circle", x: 200, y: 540, radius: 200, fillColor: "#ff3c00", opacity: 20, startTime: 0, duration: 10, trackId: 2 },
  { type: "circle", x: 1700, y: 300, radius: 300, fillColor: "#ff3c00", opacity: 15, startTime: 0, duration: 10, trackId: 3 }
]})

// 3. Add hero title with animation (single clip for precision)
craft_clip({ clip: { 
  type: "text", x: 960, y: 500, text: "ELITE FITNESS", 
  fontSize: 120, fontFamily: "Impact", color: "#ffffff", align: "center",
  startTime: 0.5, duration: 4, trackId: 4,
  fx: [{ type: "animation", name: "fadeIn", duration: 0.5, position: "in" }]
}})

// 4. Add tagline (single clip, appears after title)
craft_clip({ clip: { 
  type: "text", x: 960, y: 620, text: "TRANSFORM YOUR BODY", 
  fontSize: 36, fontFamily: "Arial", color: "#ff3c00", align: "center",
  startTime: 1.5, duration: 3, trackId: 5,
  fx: [{ type: "animation", name: "slideUp", duration: 0.4, position: "in" }]
}})

// 5. Add end screen elements in batch (appear together at the end)
craft_clips({ clips: [
  { type: "rect", x: 660, y: 750, width: 600, height: 80, fillColor: "#ff3c00", cornerRadius: 8, startTime: 3, duration: 2, trackId: 6 },
  { type: "text", x: 960, y: 805, text: "SUBSCRIBE NOW", fontSize: 32, color: "#ffffff", align: "center", bold: true, startTime: 3, duration: 2, trackId: 7 }
]})
```

### Example: Editing Shapes
```javascript
// Add a shape (returns ID)
craft_shape({ shape: { type: "rect", x: 100, y: 100, width: 200, height: 100, fillColor: "#ff0000" }})
// Response: ✓ Shape added (rect, id: shape-1737849600000-1). Total shapes: 1

// List shapes to see IDs
list_shapes()
// Response: 1 shapes:
// [0] rect "shape-1737849600000-1" (100,100)

// Update the shape
update_shape({ id: "shape-1737849600000-1", updates: { fillColor: "#00ff00", x: 150 }})
// Response: ✓ Shape updated. Updated: fillColor, x

// Delete the shape
delete_shape({ id: "shape-1737849600000-1" })
// Response: ✓ Shape deleted. Remaining shapes: 0
```

### When to Use Each

| Scenario | Recommended Tool |
|----------|------------------|
| **Canvas** | |
| Background layers, decorative elements | `craft_shapes` (batch) |
| Main headline or hero text | `craft_shape` (single) |
| Button with label (rect + text) | `craft_shapes` (batch) |
| Adding one element after user feedback | `craft_shape` (single) |
| Complete section (header, body, footer) | `craft_shapes` (batch) |
| Testing if something works | `craft_shape` (single) |
| Modifying existing element | `update_shape` |
| Removing unwanted element | `delete_shape` |
| **Video** | |
| Background/ambient elements (same timing) | `craft_clips` (batch) |
| Hero text with specific animation | `craft_clip` (single) |
| End screen (CTA button + text) | `craft_clips` (batch) |
| Sequenced text (title → subtitle) | `craft_clip` (single, each) |
| Multiple elements appearing together | `craft_clips` (batch) |
| Fine-tuning animation timing | `update_clip` |
| Removing a clip | `delete_clip` |

### Key Principle

**Batch related elements, isolate important ones.**

- Group shapes/clips that belong together visually or temporally
- Keep hero elements separate for easier adjustments
- When debugging, switch to singular tools to isolate issues
- For video: batch clips with the same `startTime`, isolate clips with unique animations
- Use `update_shape`/`update_clip` to modify without recreating
- Use `list_shapes`/`list_clips` to find IDs before editing


## References

For detailed information, read these reference files:

- **[references/canvas-shapes.md](./references/canvas-shapes.md)** — Complete shape properties and examples
- **[references/video-timeline.md](./references/video-timeline.md)** — Temporal properties, tracks, audio, video clips
- **[references/animations.md](./references/animations.md)** — The `fx` array animation system, easing, effects

## Critical Rules

1. **Canvas mode is infinite**: Shapes can be placed anywhere; `width`/`height` are optional export metadata
2. **Video mode has fixed bounds**: All elements should stay within video dimensions for the export frame
3. **Video tracks**: Overlapping clips MUST have different `trackId` values
4. **Animations**: Use the `fx` array on shapes, NOT a `keyframes` property
5. **Media sources**: Use CORS-friendly URLs (Unsplash, Pexels, Pixabay)
6. **Centered text**: When `align: "center"`, the `x` is the center point
7. **Property names**: Use `fillColor` for fill, `color` for stroke, `lineWidth` for stroke width
8. **Shape IDs**: Shapes get auto-generated IDs; use `list_shapes` to find them for editing

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