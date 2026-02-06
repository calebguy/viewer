# CLAUDE.md

## Project Overview

Single-page Three.js 3D model viewer. Everything lives in `index.html` — no build system, no framework, no package manager.

## Tech Stack

- **Three.js v0.170.0** loaded via ES module import maps from CDN (jsdelivr)
- Vanilla HTML/CSS/JS — single `index.html` file
- No build tools, no transpilation, no bundler

## Running

Serve the directory with any static file server:
```bash
npx serve .
# or
python3 -m http.server
```
Then open in a browser. The app loads `totem.glb` as the default model.

## Architecture

The entire app is in `index.html`:
- **Lines 1–112**: HTML structure + CSS (dark UI, glassmorphism panels)
- **Lines 113–233**: HTML controls (model, camera, light panels) + import map
- **Lines 234–662**: ES module JavaScript (Three.js scene, loaders, controls, animation loop)

Key components:
- **Scene setup**: PerspectiveCamera, WebGLRenderer with ACES filmic tone mapping, OrbitControls, RoomEnvironment for PBR reflections
- **Model loading**: GLTFLoader and OBJLoader; models are centered/scaled to fit a normalized bounding box
- **UI panels**: Model controls (rotation, spin), Camera controls (height, distance, angle, FOV, tilt), Light controls (color, intensity, direction, ambient, exposure, env map)
- **Persistence**: All settings saved to `localStorage` under key `viewer-settings`
- **Skydome**: Optional background image mapped to a large inverted sphere

## Assets

Binary assets (`.glb`, `.obj`, `.png`) are gitignored. The default model `totem.glb` must be present locally for the app to work.

## Key Behaviors

- **H key** toggles all UI panels
- **Color swatch** overrides model material; **double-click** restores original textures
- **Import Model** button accepts `.glb`, `.gltf`, `.obj` files
- **Dome Image** sets a skydome background image (stored as data URL in localStorage)
- Spin defaults to Y-axis; configurable per-axis with speed control
