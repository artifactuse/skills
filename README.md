# Artifactuse Skills

Agent Skills for the [Artifactuse](https://artifactuse.com) canvas and video editor SDK.

## Installation

```bash
npx skills add artifactuse/skills
```

Or with other tools:

```bash
# OpenSkills
npx openskills install artifactuse/skills

# add-skill (Vercel)
npx add-skill artifactuse/skills
```

## What's Included

### `artifactuse` skill

Generate static graphics (canvas mode) and animated videos (video mode) using JSON output.

**Use cases:**
- Diagrams, flowcharts, illustrations
- Social media graphics, banners, logos  
- Video intros, outros, title sequences
- Animated text, motion graphics
- Infographics and data visualizations

## Compatibility

Works with:
- Claude Code
- Cursor
- OpenAI Codex
- OpenCode
- Gemini CLI
- And 15+ other AI coding tools

## Quick Example

```video
{
  "width": 1920,
  "height": 1080,
  "currentPreset": "1080p",
  "duration": 10,
  "background": "#1a1a2e",
  "shapes": [
    {
      "type": "text",
      "x": 960,
      "y": 540,
      "text": "Welcome",
      "fontSize": 120,
      "fontFamily": "Arial",
      "fontWeight": "bold",
      "fill": "#ffffff",
      "textAlign": "center",
      "startTime": 0,
      "duration": 5,
      "fx": [
        { "type": "animation", "name": "fadeIn", "duration": 0.5, "position": "in" }
      ]
    }
  ]
}
```

## Documentation

- [SKILL.md](./skills/artifactuse/SKILL.md) — Main skill file
- [references/canvas-shapes.md](./skills/artifactuse/references/canvas-shapes.md) — Shape reference
- [references/video-timeline.md](./skills/artifactuse/references/video-timeline.md) — Timeline & tracks
- [references/animations.md](./skills/artifactuse/references/animations.md) — Animation system

## License

MIT
