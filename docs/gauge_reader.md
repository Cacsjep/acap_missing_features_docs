# Gauge Reader

The Gauge Reader analyses a video stream to read the position of analog gauge needles. Each detected reading is converted to a numeric value (using the gauge's min/max range and unit) and emitted as an AXIS event so it can be used by other systems.

---

## Overview

Place one or more virtual gauges over the live video. For each gauge you configure the pivot, the search ring (inner and outer radius), the angular sweep (min and max angle) and the value range. The system reads the current needle angle every frame, smooths it, and fires an AXIS event when the reading drifts past a configurable change threshold.

**Use cases:**

- Industrial pressure, temperature or flow gauges that have no digital output
- Retrofitting analog dashboards into modern monitoring systems
- Periodic logging of legacy meters via the AXIS event bus or Flow

---

## Profiles

Each Gauge Reader instance can run **up to 4 independent profiles in parallel**. A profile owns its own video channel, resolution, framerate, analysis interval, preview configuration and gauge set. Profiles are useful when a device has multiple sensors/streams and each shows its own set of dials.

The profile selector is the chip row at the top of the configuration column:

| Action | How |
|--------|-----|
| Switch profile | Click any profile chip |
| Add profile | Click the **+** icon next to the chips (max 4) |
| Remove profile | Click the **×** on a profile chip (the last remaining profile cannot be removed) |
| Rename / change channel | Adjust the Stream panel while the profile is active, then **Save** |

Switching the active profile clears the live overlay (gauges, needles, status) immediately and reconnects the WebSocket to the new profile's emitter, so you do **not** see the previous profile's needles briefly during the switch.

Per-profile events are described in the **Events** section below.

---

## Getting started

1. Open **Gauge Reader** from the feature menu.
2. (Optional) Click **+** in the profile bar to add another profile bound to a different channel.
3. Drag the pivot dot to the centre of the gauge.
4. Drag the radius handles so the search ring (annulus) is just inside the dial face and just before the needle tip.
5. Drag the **MIN** and **MAX** handles to the start and end of the dial scale.
6. Set **Min value** and **Max value** to match the printed scale. Set the **Unit** if you want the live readout to show one (e.g. `bar`, `psi`).
7. Pick a **Detection mode** (see below) and run **Auto-calibrate** if you are not sure which one fits.

---

## Configuration

### Geometry

| Setting | Description |
|---------|-------------|
| Pivot | Centre of the needle (drag the yellow dot) |
| Inner radius | Excludes pixels in the centre (pivot hub or screw) |
| Outer radius | Excludes pixels outside the dial. Set roughly to the needle tip |
| Min / Max angle | Start and end of the dial sweep |
| CW / CCW | Direction the value increases along the sweep |

The grey area on the overlay shows pixels the algorithm ignores: the disc inside the inner radius (the hub) and the wedge outside the min/max sweep.

### Value range

| Setting | Description |
|---------|-------------|
| Min value | Numeric value at the **MIN** angle |
| Max value | Numeric value at the **MAX** angle |
| Unit | Optional unit string for the on-screen readout (e.g. `bar`, `psi`, `C`) |
| Decimals | Number of fraction digits in the displayed value |

### Detection mode

| Mode | When to use |
|------|-------------|
| Auto | Most robust. Runs all four other modes per frame and picks the strongest consensus. Use when conditions vary or you don't want to commit to one mode. Costs ~4x the per-frame CPU of a single mode |
| Edge | Detects the needle as a thin edge. Works even when needle and dial are similar colours |
| Dark | Needle is clearly darker than the dial face. Threshold is set automatically by Otsu's method, so it adapts to whatever contrast the dial has — including faded or low-contrast gauges |
| Color | Needle has a distinctive colour (red, orange). Pick the colour with the eyedropper |
| Contour | Best for cluttered dials with printed numbers and scale ticks. Finds the needle by tracing a connected shape that crosses both the inner and outer ring |

In **Dark** mode the threshold is derived from the luma histogram via Otsu's method (maximum inter-class variance) — it adapts to the actual dial-vs-needle contrast and handles "light needle on dark dial" cases automatically. In **Contour** mode the threshold is the annulus mean luma offset by 25 (or by half the observed luma range, whichever is smaller, so low-contrast dials still get a usable cutoff). Setting **Luma threshold** to a non-zero value overrides both.

### Smoothing and events

| Setting | Description |
|---------|-------------|
| Smoothing | 0 reacts immediately, 0.95 is heavily damped. Higher values reduce jitter at the cost of latency |
| Change threshold | Minimum value drift (in the configured unit) before a new event is fired |

The change threshold is measured against the value at the **last emitted** event, not the previous frame. Without that, low-confidence readings could re-fire every frame even if the needle is physically still.

### Auto-calibrate

The **Auto-calibrate** button runs the current frame through every detection mode and a sweep of thresholds, scoring each combination by peak strength and matched-pixel ratio. The best scoring configuration is written back to the gauge **and persisted immediately** — no separate Save click is required, and the calibration survives a service restart. Geometry (pivot, radii, angles) is not touched: that part is set visually with the overlay.

Auto-calibrate picks one specific mode (Edge, Dark, Color, or Contour) — the one that scored highest on the current frame. If you want the reader to keep choosing per frame instead of locking in one mode, switch the mode to **Auto** afterwards.

---

## How it works

1. Each frame is read from the video pipeline as NV12 luma plus chroma.
2. Pixels in the search ring (between inner and outer radius, inside the min/max sweep) are classified as "needle" using the chosen detection mode.
3. The classified pixels feed an angular histogram. The histogram is smoothed and divided by the per-bin annulus density so a thicker scale arc does not get mistaken for a peak.
4. A weighted PCA on the matched pixels around the peak refines the angle below 1 degree resolution.
5. **Contour mode** runs a connected-components pass on the matched pixels and picks the largest component that touches both the inner and outer ring. Printed numbers and scale ticks fail that "crosses both rings" test by construction.
6. **Auto mode** runs Edge, Dark, Contour, and (if a needle colour is configured) Color in turn, then picks a consensus answer: the largest cluster of modes whose predicted angles agree within 5°, with the highest-trust mode in that cluster as the final reading. If no two modes agree, the highest-trust mode overall wins. On synthetic data this reaches 98% within-2° on a hostile distribution that includes counterweights, half-needles, perspective tilt, low-contrast dials, glare, noise, blur, scale-text clutter, and intruding ticks.
7. The angle is mapped to a numeric value using the configured min/max range, smoothed, and compared against the change threshold.

### Hold-last-good

If a single frame fails to produce a reading (motion blur, brief occlusion, focus hunt) the previous reading is kept for up to 2 seconds. While held, the on-screen needle dims and switches to a dashed stroke so the operator can see the value is not freshly measured.

### Confidence

| Range | Meaning |
|-------|---------|
| 1.0 | Strong, decisive peak |
| 0.5 to 0.9 | Plausible, may benefit from a tighter ring or a different mode |
| Below 0.3 | Noise dominates. Consider switching detection mode or adjusting the threshold |

---

## Events

### Per-gauge event

Each gauge fires its own AXIS event when the value crosses the change threshold.

**Event name:** `{gauge_label}_state_{profile_name}` (for example `gauge1_state_profile_1`)

**Event data:**

| Field | Description |
|-------|-------------|
| profile_name | Name of the profile that emitted the event |
| gauge_name | Display name of the gauge |
| value | Current smoothed value (numeric) |
| raw_angle | Detected needle angle in degrees |
| confidence | Detection confidence between 0 and 1 |

The unit is **not** carried in the event payload (it would inject non-ASCII bytes for symbols like the degree sign and break some ONVIF consumers). Pair the value with the unit configured in the UI on the receiving side.

### Combined event

A single combined event fires per profile whenever any gauge in that profile changes:

| Event name | Description |
|------------|-------------|
| `all_gauges_state_{profile_name}` | Summary of every configured gauge in the profile |

**Combined event data:**

| Field | Description |
|-------|-------------|
| profile_name | Name of the profile that emitted the event |
| count | Number of gauges in the report |
| `gauge_{N}_value` | Current value of gauge N (1-indexed) |

The `_{profile_name}` suffix on the event name means each profile has its own combined event so subscribers can route events per stream.

### Using events in AXIS rules

While the Gauge Reader is running, events appear in the AXIS event system:

1. Open the device's web interface
2. Go to **System > Events**
3. Find events under the Missing Features ACAP
4. Create a rule triggered by a gauge state change

### Using events in Flow

Use the **Axis Metadata Event (Subscribe)** node:

1. Add the subscribe node to the flow
2. Pick the per-gauge event or `all_gauges_state`
3. Wire the value field into downstream logic

---

## Live preview

The preview shows:

- Real-time video via WebCodecs
- Overlay handles for pivot, radii and min/max angles
- The current detected needle as a coloured line. Solid red means a fresh reading. Dashed orange means a held value during the hold-last-good window.
- A small per-gauge readout with value, unit and confidence

### Controls

| Button | Action |
|--------|--------|
| Add gauge | Create a new virtual gauge |
| Auto-calibrate | Sweep detection modes and thresholds, pick the best |
| Show / Hide | Toggle the overlay |
| Fullscreen | Expand the preview to fill the screen |
| Help | Open the built-in help dialog |

---

## Best practices

### Geometry

- Keep the inner radius just outside the pivot hub. The hub matches in every direction and washes out the angular peak if it is included.
- Keep the outer radius just inside the printed numbers. Numbers and ticks are the most common false-positive source.
- Set min / max angle from the printed scale, not the physical end-stops of the needle. The end-stops often sit a few degrees outside the printed range.

### Detection mode

- If you don't know which mode fits, pick **Auto**. It runs all four every frame and picks the strongest consensus. CPU cost is roughly 4x a single mode but on a typical analysis interval (≥ 200 ms) this is cheap.
- For lowest CPU cost: try **Edge** first. It tolerates same-colour needles and dials better than the threshold modes.
- Use **Contour** on busy dials with lots of printed text inside the ring.
- Use **Color** only when the needle is brightly coloured and the dial is not.
- Use **Dark** when the needle is plainly darker than the dial. The new Otsu threshold makes Dark mode work on low-contrast dials too — you no longer need to switch to Edge for faded gauges.

### Stability

- Increase **Smoothing** if the value visibly jitters even when the needle is still.
- Increase **Change threshold** to reduce event volume. The threshold is in the gauge's own unit, so 1.0 on a 0 to 100 bar gauge means "1 bar of drift".
- Lower confidence readings often mean the search ring is too wide. Tighten it before changing detection modes.

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| No needle detected | Run Auto-calibrate. If it still fails, check that the search ring covers the needle and the min/max sweep matches the printed scale |
| Needle points to the wrong side of the dial | Flip the CW / CCW setting |
| Reading bounces several units between frames | Increase Smoothing, or tighten the search ring so background noise stops contributing |
| Events fire on every frame | Increase Change threshold. The threshold is compared to the last emitted value, so a value of 0 emits on every numeric jitter |
| Diagnostic image shows lots of red | Lower the Luma threshold (in Dark or Contour mode), or switch to Edge mode |
| "Light needle on dark dial" not detected | Leave Luma threshold at 0. The auto mode flips polarity for dark dials |
