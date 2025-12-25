# Timing Nodes

Nodes for time-based logic, counting, and delay generation.

---

## Timer on Delay (TON)

The **Timer on Delay** outputs true only after the input has been continuously true for the configured duration.

**Category**: Time, Edge

### Functionality

When `Enable` goes true, the timer starts counting. If `Enable` stays true for the full duration, `Active` becomes true. If `Enable` goes false before the time elapses, the timer resets.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Enable | Boolean | Starts the timer when true |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Active | Boolean | True after delay completes |
| Elapsed Time | Integer | Milliseconds counted so far |

### Properties

- **Amount**: Time value
- **Unit**: ms, seconds, or minutes

### Use Cases

- Filter brief sensor triggers
- Require sustained conditions before acting
- Implement "hold to confirm" patterns

---

## Timer off Delay (TOF)

The **Timer off Delay** goes true immediately when enabled, then stays true for a configured time after the input goes false.

**Category**: Time, Edge

### Functionality

When `Enable` goes true, `Active` immediately becomes true. When `Enable` goes false, the timer starts. `Active` remains true until the delay expires.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Enable | Boolean | Triggers the timer |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Active | Boolean | True during input and delay |
| Elapsed Time | Integer | Milliseconds in delay phase |

### Use Cases

- Keep lights on after motion stops
- Extend relay activation after trigger ends
- Smooth intermittent signals

---

## Pulse Timer (TP)

The **Pulse Timer** emits a fixed-length pulse on each rising edge of the input.

**Category**: Time, Edge

### Functionality

Each rising edge of `Enable` starts a pulse of the configured duration. The pulse length is fixed regardless of how long the input stays true.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Enable | Boolean | Rising edge triggers pulse |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Active | Boolean | True during pulse |
| Elapsed Time | Integer | Milliseconds into pulse |

### Use Cases

- Generate consistent pulse widths
- Trigger fixed-duration actions
- Normalize varying input signals

---

## Pulse Generator

The **Pulse Generator** produces periodic single-cycle pulses while enabled.

**Category**: Time, Edge

### Functionality

While `Enable` is true, outputs a one-cycle true pulse at regular intervals based on the configured period.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Enable | Boolean | Enables pulse generation |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Active | Boolean | Periodic pulse output |

### Properties

- **Amount**: Time between pulses
- **Unit**: ms, seconds, or minutes

### Use Cases

- Periodic polling triggers
- Heartbeat signals
- Timed sampling

---

## Timer Sequence

The **Timer Sequence** node activates a series of outputs one after another.

**Category**: Time, Edge

### Functionality

When `Enable` is true, it cycles through configured timers. Each timer keeps its corresponding output high for its duration, then hands off to the next. When all timers complete, it wraps back to the first.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Enable | Boolean | Starts the sequence (false resets) |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Elapsed Time | Integer | Time in current slot (ms) |
| Timer Index | Integer | Current active timer (0-based) |
| Timer 1, Timer 2... | Boolean | Active during respective slot |

### Configuration

1. **Add Timer**: Use Properties panel to add steps
2. **Duration**: Set length and unit for each step
3. **Label**: Rename outputs (e.g., "Red", "Yellow", "Green")

### Use Cases

- Traffic light sequences
- Multi-stage processes
- Timed state machines

---

## One Shot Pulse After

The **One Shot Pulse** node generates a delayed pulse.

**Category**: Time, Edge

### Functionality

On a rising edge at `Trigger`, waits for the configured delay, then emits a single-cycle true pulse.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Trigger | Boolean | Rising edge starts delay |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Pulse | Boolean | Delayed output pulse |
| Elapsed Time | Integer | Time waited so far (ms) |

### Use Cases

- Delayed reactions
- Scheduled triggers
- Timeout implementations

---

## Debounce

The **Debounce** node filters noisy boolean signals.

**Category**: Logic, Time

### Functionality

The output only changes when the input has been stable for the configured duration. This eliminates bouncing or flickering.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Input | Boolean | Signal to debounce |
| Reset | Boolean | Clears the timer |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Boolean | Debounced output |
| Elapsed | Integer | Time input has been stable (ms) |

### Properties

- **Amount**: Stability time required
- **Unit**: ms, seconds, or minutes

### Use Cases

- Physical button inputs
- Noisy sensor signals
- Unstable network states

---

## Counter

The **Counter** node tracks a number based on input pulses.

**Category**: Memory, Edge

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Up | Boolean | Rising edge increments |
| Down | Boolean | Rising edge decrements |
| Reset | Boolean | Sets count to zero |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Integer | Current count |

### Use Cases

- Event counting
- Occupancy tracking
- Cycle counting

---

## Step Sequencer

The **Step Sequencer** advances through outputs on each trigger pulse.

**Category**: Memory, Edge

### Functionality

Each rising edge of `Step` advances to the next output. Only one output is true at a time. After the last step, wraps to the first.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Step | Boolean | Rising edge advances |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Step 1...N | Boolean | Current step indicator |

### Properties

- **Steps**: Number of outputs (2-16)

---

## Measure Time

The **Measure Time** node measures duration between events.

**Category**: Utility

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Start | Boolean | Rising edge starts timer |
| Stop | Boolean | Rising edge stops timer |
| Reset | Boolean | Clears the measurement |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Duration ms | Integer | Measured time in milliseconds |
| Running | Boolean | True while measuring |
| Done | Boolean | Pulses true when stopped |

### Use Cases

- Process timing
- Response time measurement
- Performance monitoring

---

## In Time

The **In Time** node outputs true during a configured daily time window.

**Category**: Time

### Functionality

Compares current time against a configured schedule. Outputs true when within the time range on allowed days.

### Outputs

| Name | Type | Description |
|------|------|-------------|
| In Time | Boolean | True during configured window |

### Properties

- **Start Time**: Beginning of window (e.g., 08:00)
- **End Time**: End of window (e.g., 17:00)
- **Days**: Which days of week are active

### Use Cases

- Business hours detection
- Scheduled operations
- Time-based access control
