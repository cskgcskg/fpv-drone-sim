# 🐝 FPV Drone Simulator v7 Ultra

**A realistic, browser-based FPV drone simulator with cinematic visuals, physics-driven flight, and immersive post-processing — inspired by The Zone drone sim.**

> No install. No backend. Just open `index.html` in Chrome and fly.

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)
![Three.js](https://img.shields.io/badge/Three.js-r128-000000?style=flat&logo=three.js)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-F7DF1E?style=flat&logo=javascript&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-green.svg)

---

## ✈️ Features

### 🎮 Flight Physics (Betaflight-Inspired)
- **PID-only angular control** — single rotation authority, matching real Betaflight firmware
- **Motor spool model** — lag on acceleration, active braking on deceleration
- **Prop wash simulation** — thrust loss and angular instability in descent
- **Gyroscopic precession** — spinning prop effects on roll/pitch coupling
- **Parasitic yaw** — throttle-change CW/CCW torque imbalance
- **Body-frame directional drag** — different coefficients for side, vertical, and forward movement
- **Ground effect** — increased thrust efficiency near the ground
- **Battery voltage sag** — performance degrades under load over time
- **200Hz physics sub-stepping** — stable simulation regardless of frame rate

### 🌬️ Dynamic Wind System
- Persistent wind direction with slow drift
- Randomized gusting with decay
- High-frequency turbulence layer
- Cross-body torque (weathervaning)
- Affects dust particles, wind sock, and disarmed drone drift

### 🎬 Cinematic Post-Processing (Single-Pass GPU Shader)
- **Chromatic aberration** — subtle RGB split that intensifies with speed
- **Radial motion blur** — lightweight, speed-reactive
- **Vignette** — warm-tinted oval darkening, tightens at high speed
- **Film grain** — temporal noise overlay
- **Scanline overlay** — subtle CRT-style analog feel

### 📷 Camera System
- **Smooth FOV transitions** — lerped field-of-view widens with speed
- **Layered camera shake:**
  - Ground rumble (contact vibration)
  - High-speed vibration (airframe resonance)
  - Motor micro-vibration (prop frequency)
- **All shake is smoothly decayed** — no jitter, no sudden jumps

### 🖥️ Immersive FPV HUD
- SVG crosshair reticle
- Telemetry cards: ALT, SPD, BATT (with bar), THR, TIME
- Wind direction arrow + speed readout
- Altitude tape (right side)
- Throttle bar (left side)
- Dual stick position indicators
- Semi-transparent, non-intrusive design

### 🌍 Environment
- Procedural atmospheric sky with Rayleigh scattering, sun disc, corona, and god rays
- 50 volumetric cloud planes with drift and bob
- Animated water shader with Fresnel reflection and wave normals
- Textured terrain with rolling hills (flat near launch pad)
- 800 instanced grass blades (single draw call)
- 100 mixed deciduous/conifer trees
- 12 detailed buildings with windows, rooftop AC units, and aviation lights
- Racing gates with glow rings
- Shipping containers, parked vehicles, fences, benches
- Power lines with catenary wire sag
- Radio tower with blinking beacon
- Circling bird silhouettes
- Atmospheric dust/pollen particles
- Prop wash dust near ground

### 🎮 Input Support
- **Gamepad** — full stick calibration wizard, axis mapping, inversion toggles
- **Keyboard** — WASD (throttle/yaw) + Arrow keys (pitch/roll), Mode 2 layout
- **Smooth input interpolation** — eliminates jitter on both input methods
- **DJI FPV RC3 tested** — USB-C wired connection

### 🚁 Two Drone Profiles
| | Bee35 (Cinewhoop) | Mario 5 (Freestyle) |
|---|---|---|
| Weight | 500g | 680g |
| Props | 3.5" | 5.1" |
| TWR | 5:1 | 9:1 |
| Feel | Smooth, forgiving | Raw power, snappy |
| Top Speed | ~100 km/h | ~150+ km/h |

Hot-swap drones mid-flight with `1` / `2` keys.

### ⚙️ In-Flight Settings (TAB)
- Weight, power, drag
- Rates (°/s), expo curve
- Camera angle
- Live TWR display

---

## 🚀 Quick Start

1. **Download** `index.html`
2. **Open** in Chrome (or any modern browser)
3. **Choose drone** → select controller or keyboard → Fly!

**Keyboard controls:**
| Key | Action |
|-----|--------|
| `SPACE` | Arm / Disarm |
| `W` / `S` | Throttle up / down |
| `A` / `D` | Yaw left / right |
| `↑` / `↓` | Pitch forward / back |
| `←` / `→` | Roll left / right |
| `R` | Reset position |
| `TAB` | Settings panel |
| `H` | Help overlay |
| `1` / `2` | Switch drone |
| `C` | Recalibrate controller |

---

## 🔧 Technical Details

### Architecture
The simulator is built as a single HTML file with modular ES6 classes:

| Class | Responsibility |
|-------|---------------|
| `Wind` | Dynamic wind simulation (direction, gusting, turbulence) |
| `Setup` | Controller detection, calibration wizard, drone selection |
| `Input` | Gamepad/keyboard abstraction with smooth interpolation |
| `Phys` | 200Hz physics engine (forces, collisions, PID angular control) |
| `Rend` | Three.js scene, environment, post-processing pipeline |
| `OSD` | HUD telemetry updates |
| `Aud` | Web Audio API motor sound synthesis |
| `Sett` | Runtime settings panel |
| `SpeedFX` | Canvas-based speed line overlay |
| `Sim` | Main loop coordinator |

### Performance Optimizations
- **Instanced mesh** for grass (800 blades, 1 draw call)
- **Single-pass post-processing** shader (chromatic aberration + motion blur + vignette + grain in one fragment shader)
- **Shadow camera follows drone** — consistent quality without massive shadow maps
- **`powerPreference: 'high-performance'`** WebGL context hint
- **Capped pixel ratio** at 2x to prevent GPU overload on high-DPI screens
- **Physics accumulator pattern** — fixed 200Hz timestep decoupled from render rate
- **Particle pools** — pre-allocated, recycled (no GC pressure)
- **No object creation in animation loop** — vectors reused via `.copy()` / `.set()`

### Dependencies
- [Three.js r128](https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js) (loaded from CDN)
- Google Fonts: Share Tech Mono, Rajdhani (loaded from CDN)
- **No build step. No npm. No bundler.**

### Browser Support
- ✅ Chrome (recommended)
- ✅ Firefox
- ✅ Edge
- ✅ Safari (WebGL required)
- ⚠️ Gamepad API may not work in iframes — open the .html file directly

---

## 📁 Repository Structure

```
fpv-drone-sim/
├── index.html    # Complete simulator (single file)
├── README.md                 # This file
└── LICENSE                   # MIT License
```

---

## 🤝 Credits & Acknowledgments

- **Physics model** inspired by [The Zone](https://www.ofrthezone.com/) drone simulator's approach of tuning from real Betaflight blackbox logs
- **AI-assisted development** — significant portions of this codebase were developed with the assistance of **Claude (Anthropic)**, including the post-processing pipeline, wind system, HUD redesign, instanced geometry, camera shake system, and overall code architecture
- **Three.js** — [mrdoob/three.js](https://github.com/mrdoob/three.js) for the WebGL rendering engine
- **SpeedyBee** — drone specs referenced from the Bee35 and Mario 5 product lines

---

## 📄 License

MIT License — use it, fork it, modify it, fly it.

---

## 🛠️ Contributing

Contributions welcome! Some ideas for future work:

- [ ] Freestyle trick detection (flips, rolls, powerloops)
- [ ] Lap timer for racing gates
- [ ] Multiple environment maps (urban, mountain, warehouse)
- [ ] Multiplayer via WebRTC
- [ ] Replay system with camera angles
- [ ] Custom drone builder
- [ ] Mobile touch controls
- [ ] VR/WebXR headset support

---

*Built for the FPV community. Rip it.* 🏁
