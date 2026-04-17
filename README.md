# Spacecraft Flight Simulator

An interactive, single-file web simulator for interplanetary and interstellar spacecraft missions. No server or build step required — open `index.html` in any modern browser.

![dark space theme with velocity and position charts](https://via.placeholder.com/800x400/05080f/38bdf8?text=Spacecraft+Flight+Simulator)

## Features

- **Classical & relativistic physics** — automatically applies special relativity when speeds approach *c*
- **Two propulsion models**: chemical/nuclear rocket (Tsiolkovsky) and laser sail (photon pressure)
- **8 pre-built mission presets** across three ship classes
- **Live charts** — velocity, position, and Lorentz factor (γ) vs. time, with speed-of-light reference line
- **Spaceship cursor** on chart hover
- **Mobile-friendly** — responsive two-column → single-column layout
- **Zero dependencies** beyond Chart.js 4.4.0 (loaded from CDN)

---

## Quick Start

```
git clone https://github.com/<you>/spaceship-simulator.git
cd spaceship-simulator
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux
```

Or just double-click `index.html`.

---

## Mission Presets

### Racer-X — Chemical / Nuclear Rocket
| Preset | Destination | Mode | Notes |
|---|---|---|---|
| Mars Sprint | Mars (0.52 AU) | Brachistochrone | Constant 1 g flip-and-burn |
| Mars Dart | Mars (0.52 AU) | Dart | Burn at start only, coast the rest |
| Neptune | Neptune (29.8 AU) | Brachistochrone | Intermediate thrust, 3 months |
| Neptune Dart | Neptune (29.8 AU) | Dart | Burn + coast |
| Proxima 1 g | Proxima Centauri (4.24 ly) | Brachistochrone | Full relativistic — trip takes ~3.5 yr ship time |

### Laser-X — Laser Sail (Photon Pressure)
| Preset | Payload | Laser Power | Sail Area | Destination |
|---|---|---|---|---|
| CubeSat | 10 kg | 30 GW | 100 m² | Mars |
| Probe | 800 kg | 2.4 TW | 8 000 m² | Neptune |
| Crewed | 20 000 kg | 60 TW | 200 000 m² | Proxima Centauri |

---

## Physics Models

### Rocket Propulsion

Uses the **Tsiolkovsky rocket equation**:

```
Δv = Isp × g₀ × ln(m₀ / m₁)
```

Two flight profiles:

- **Brachistochrone** — constant thrust the whole trip; flip at midpoint to decelerate.  
  `t_total = 2 × √(d / a)`,  `v_max = √(a × d / 2)`

- **Dart** — burn once at departure, coast to destination.  
  `t_total = √(2d / a)`

### Laser Sail Propulsion

Photon thrust from a ground-based (or orbital) laser:

```
F = (1 + R) × P / c        (R = reflectivity, 0 = absorber, 1 = mirror)
a = F / m
```

**Beam divergence cutoff** — the laser beam spreads over distance. Once the beam footprint exceeds the sail, effective power falls as:

```
P_eff = P × (d_cutoff / x)²       for x > d_cutoff
d_cutoff = r_sail / θ_divergence
```

An amber annotation on the chart marks where the laser cuts off, and a plain-English explanation card appears below the charts.

### Special Relativity

When `v/c > 1 %`, the simulator switches to relativistic kinematics:

```
dv/dt = a / γ³            (coordinate acceleration)
dτ    = dt / γ            (proper time accumulates slower)
γ     = 1 / √(1 − v²/c²)
```

The **γ chart** shows the Lorentz factor over the mission. A dashed amber line marks *c* on the velocity chart.

---

## Controls Reference

| Control | Description |
|---|---|
| Propulsion tabs | Switch between Rocket and Laser Sail parameter panels |
| Distance (AU / ly) | Mission distance — overridden by presets |
| Acceleration (g) | Thrust acceleration in multiples of Earth gravity |
| Isp (s) | Specific impulse — higher = more propellant-efficient |
| Mass ratio m₀/m₁ | Wet mass / dry mass — drives Δv budget |
| Infinite propellant | Ignore propellant mass (useful for upper-bound estimates) |
| Laser power (GW/TW) | Total beam power at source |
| Sail area (m²) | Capture area of the reflective sail |
| Reflectivity R | 0 = perfect absorber, 1 = perfect mirror |
| Divergence (µrad) | Beam half-angle; drives cutoff distance |
| Run Simulation | Compute and render results |

---

## File Structure

```
spaceship-simulator/
└── index.html     # entire app — HTML + CSS + JS in one file
└── README.md
```

No build tools, no npm, no bundler. The only external resource is Chart.js:

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
```

---

## Browser Support

Chrome 90+, Firefox 88+, Safari 14+, Edge 90+. Works on iOS and Android (portrait and landscape).

---

## License

MIT
