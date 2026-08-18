# 3D Room Viewer (Three.js Standalone Viewer)

A lightweight, browser-based 3D room viewer built with **Three.js**. This application renders pre-baked 3D scenes complete with PBR materials, custom studio dynamic lighting, toon/cel shading support, interactive trigger zones, and embedded video player capabilities.

---

## 🎨 Features

* **First-Person Controls:** Smooth FPS-style movement (WASD + Mouse Look) using Pointer Lock API.
* **Realistic Dynamic Lighting:** Studio 3-point light setup (Key, Fill, and Rim lights) paired with standard hemisphere lighting and soft shadow mapping.
* **Environment Reflections:** Integrated `RoomEnvironment` PMREM Generator to supply metallic and glossy surfaces with life-like reflections.
* **Toon / Cel Shading Support:** Built-in dynamic toon material conversion, custom stepped gradients, and inverted-hull edge outlining.
* **Embedded Ink Strokes:** Renders 3D hand-drawn ink strokes via `TubeGeometry` parsed directly from scene data.
* **Interactive Trigger Zones:** Proximity detection triggers customizable HTML modal overlays (e.g., info panels, popups) via `iframe`.
* **Embedded Video & Audio:** Supports 3D spatial video planes embedded in the environment with auto-unlocking browser audio on user interaction.
* **Zero Build Pipeline:** Standard HTML5 and WebGL ES modules—runs directly in modern web browsers without complex build tools.

---

## 🎮 Controls

| Action | Control |
| :--- | :--- |
| **Look Around** | Click canvas (Enables Mouse Look) |
| **Move** | <kbd>W</kbd> <kbd>A</kbd> <kbd>S</kbd> <kbd>D</kbd> |
| **Interact / Open Trigger** | <kbd>E</kbd> |
| **Close Overlay / Release Mouse** | <kbd>Esc</kbd> |

---

## 📁 File Structure

```text
.
├── index.html          # Main application file containing scripts and baked JSON scene data
├── html0.html          # (Optional) HTML content loaded into trigger overlay 0
├── html1.html          # (Optional) HTML content loaded into trigger overlay 1
└── README.md           # Project documentation
```

---

## ⚙️ How It Works

1. **Scene Initialization:**
   The viewer sets up a `WebGLRenderer` configured with ACESFilmic tone mapping, PCF soft shadow maps, and sRGB color space.

2. **Data Parsing:**
   At startup, the script searches for a `<script id="savedSceneData" type="application/json">` block containing:
   * **Objects:** Base64 encoded GLTF/GLB models, scale, positional data, brightness adjustments, solid/collision states, and toon-shading toggles.
   * **Videos:** Video source URLs/filenames, dimensions, transform properties, and loop settings.
   * **Triggers:** Position radius coordinates linked to external HTML files (`html0.html`, `html1.html`, etc.).

3. **Collision & Trigger Loops:**
   * **Obstacle Collisions:** Calculates 2D bounding boxes against the player's radius (`PLAYER_R`) to prevent passing through solid objects.
   * **Trigger Overlays:** Constantly measures distance from player to active trigger origins, popping up standard interactive prompts when nearby.

---

## 🚀 Getting Started

### Prerequisites

Since the viewer imports standard ES Modules via standard CDN (Three.js `v0.164.1`) and loads base64 textures/models, it is best served using a simple local HTTP server to avoid CORS issues.

### Running Locally

Using **Python** (Built-in):
```bash
# Python 3.x
python -m http.server 8000
```

Using **Node.js** (`serve` or `http-server`):
```bash
npx serve .
```

Open your browser and navigate to `http://localhost:8000`.

---

## 🛠️ Customizing Embedded Data

To modify the room contents without touching the core engine code, edit the JSON inside the `<script id="savedSceneData" type="application/json">` tag at the bottom of the HTML file:

```json
{
  "objects": [
    {
      "name": "example_model",
      "position": [0, 0, 0],
      "rotation": [0, 0, 0],
      "scale": 1.0,
      "brightness": 1.0,
      "solid": true,
      "toon": false,
      "glbBase64": "..."
    }
  ],
  "videos": [],
  "triggers": [
    {
      "x": 2.0,
      "z": -3.0,
      "radius": 1.5,
      "text": "Press E to inspect artwork",
      "fileIndex": 0
    }
  ]
}
```

---

## 📦 Dependencies

* [Three.js](https://threejs.org/) (`v0.164.1`) via CDN importmap.
* `GLTFLoader` standard Three.js addon.
* `RoomEnvironment` standard Three.js environment mesh.
