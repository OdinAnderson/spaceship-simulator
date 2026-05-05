# 🚀 Spacecraft Flight Simulator

An interactive, single-page simulator for **interplanetary and interstellar missions** —
combining classical kinematics, the rocket equation, special relativity, and laser-sail
propulsion in one browser tool.

> **Live:** https://odinanderson.github.io/spaceship-simulator/

No server, no build step, no `node_modules`. Open `index.html` in any modern browser.

---

## What it does

Pick (or build) a mission, dial in your engine and craft, and the simulator computes the
full trajectory and reports back:

- **Travel time** — Earth-frame and ship-frame (with time dilation when relativistic
  physics is on).
- **Peak velocity** — as a fraction of `c`, with Lorentz factor γ.
- **Δv budget** — checked against the rocket equation; warns when the mission is
  propellant-infeasible.
- **Telemetry charts** — distance, velocity, acceleration, and γ over time, rendered
  with Chart.js, including a dashed `c` reference line and a spaceship cursor on hover.

---

## Features at a glance

- **Classical & relativistic physics** — automatically switches to SR when `v/c > 1%`.
- **Two propulsion models** — chemical / nuclear rocket (Tsiolkovsky) and laser sail
  (photon pressure with beam-divergence cutoff).
- **Pre-built mission presets** across four ship classes.
- **Live charts** — velocity, position, acceleration, and Lorentz factor γ vs. time.
- **Mobile-friendly** — responsive two-column → single-column layout.
- **Hover tooltips** on every input explaining what it is and how it affects the result.
- **Zero dependencies** beyond Chart.js 4.4.0 from CDN.

---

## Mission classes

Presets are organized by propulsion class so you can compare what's actually possible
with each technology:

| Class       | Tech                       | Example presets                                   |
|-------------|----------------------------|---------------------------------------------------|
| **Racer-X** | Chemical / nuclear-thermal | Mars Close Approach, Mars Far Approach            |
| **Racer-Y** | Fusion torch               | Neptune Brachistochrone, Neptune Dart             |
| **Racer-Z** | Hypothetical 1g drive      | Proxima Centauri (1g constant burn)               |
| **Laser-X** | Beamed-energy lightsail    | CubeSat → Mars, Probe → Neptune, Crew → Proxima   |

---

## Mission modes

- **Brachistochrone** — accelerate to the midpoint, flip, decelerate to a stop at the
  destination. The "fast and arrive" profile.
  `t_total = 2·√(d/a)`,  `v_max = √(a·d/2)`
- **Dart** — accelerate the whole way (or burn once and coast). Shortest travel time,
  but you scream past the target.
  `t_total = √(2d/a)`

---

## Physics

### Rocket propulsion

Classical mode uses constant-acceleration kinematics with the **Tsiolkovsky rocket
equation** enforcing a propellant ceiling:

```
Δv = Isp · g₀ · ln(m₀ / m₁)
```

The simulator warns when a mission's required Δv exceeds the budget. An
**infinite-propellant toggle** bypasses this for "what if" drives (fusion, antimatter,
photon) — required for any 1g interstellar run.

### Laser sail propulsion

Photon thrust from a ground-based or orbital laser:

```
F = (1 + R) · P / c        (R = reflectivity: 0 = absorber, 1 = mirror)
a = F / m
```

**Beam divergence cutoff** — the laser beam spreads over distance. Once the footprint
exceeds the sail, effective power falls as:

```
P_eff    = P · (d_cutoff / x)²    for x > d_cutoff
d_cutoff = r_sail / θ_divergence
```

An amber annotation on the chart marks where the laser cuts off, with a plain-English
explanation card below.

### Special relativity

When `v/c > 1%` the simulator switches to relativistic kinematics:

```
dv/dt = a / γ³            (proper-acceleration integration)
dτ    = dt / γ            (ship-frame proper time)
γ     = 1 / √(1 − v²/c²)
```

The γ chart shows the Lorentz factor over the mission; a dashed amber line marks `c`
on the velocity chart.

---

## Controls reference

| Control               | Description                                                        |
|-----------------------|--------------------------------------------------------------------|
| Propulsion tabs       | Switch between Rocket and Laser Sail parameter panels              |
| Distance (AU / ly)    | Mission distance — overridden by presets                           |
| Acceleration (g)      | Thrust acceleration in multiples of Earth gravity                  |
| Isp (s)               | Specific impulse — higher = more propellant-efficient              |
| Mass ratio m₀/m₁      | Wet mass / dry mass — drives Δv budget                             |
| Infinite propellant   | Ignore propellant mass (useful for upper-bound estimates)          |
| Laser power (GW/TW)   | Total beam power at source                                         |
| Sail area (m²)        | Capture area of the reflective sail                                |
| Reflectivity R        | 0 = perfect absorber, 1 = perfect mirror                           |
| Divergence (µrad)     | Beam half-angle; drives cutoff distance                            |
| Run Simulation        | Compute and render results                                         |

---

## Tech & file structure

- One `index.html` — HTML + CSS + JS in a single file.
- Vanilla JavaScript, no bundler, no npm.
- [Chart.js 4.4.0](https://www.chartjs.org/) loaded from CDN for telemetry plots.
- Dark space-themed UI with a CSS star field.

```
spaceship-simulator/
├── index.html     # entire app
└── README.md
```

The only external resource:

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
```

---

## Run locally

```pwsh
# Just open it
Start-Process index.html

# Or serve over HTTP
python -m http.server 8000
# then visit http://localhost:8000/
```

On other platforms:

```
open index.html          # macOS
xdg-open index.html      # Linux
```

---

## Browser support

Chrome 90+, Firefox 88+, Safari 14+, Edge 90+. Works on iOS and Android in both
portrait and landscape.

---

## Status

`v0.1` — functional, opinionated, intentionally first-order. Issues and PRs welcome.

## License

MIT.
