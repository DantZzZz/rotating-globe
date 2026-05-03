# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Project

No build system. Open `rotating-globe.html` directly in any modern browser (WebGL required).

To develop: edit the HTML file, refresh browser. To deploy: copy the single file to any web server.

## Architecture

Single-file HTML app (`rotating-globe.html`). All logic, styles, and a base64-encoded equirectangular world map are embedded.

**Dependencies (CDN):**
- Three.js v0.160.0 — 3D rendering
- gifenc v1.0.3 — animated GIF export

**Scene graph:**
```
Scene
├── Ambient Light
├── Directional Light
└── TiltGroup
    └── Mesh (SphereGeometry 96×64 segments, MeshStandardMaterial with map texture)
```

**Key sections inside the file:**
- Init (scene, camera, renderer, texture, lighting, UI event listeners)
- Animation loop: clock-based delta time, applies tilt + rotation + mouse drag offset
- GIF export: offscreen WebGL renderer → 60 frames → gifenc → blob download

**Controls:** speed, tilt, light intensity, zoom sliders; pause button; mouse drag for free rotation.

## Replacing the World Map

1. Encode a new equirectangular image (2:1 aspect ratio) to base64
2. Replace the `MAP_DATA_URL` value in the HTML
