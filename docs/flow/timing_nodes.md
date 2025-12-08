# Timing Nodes

Nodes for time-based logic, counting, and delay generation.

## Timer Sequence
The **Timer Sequence** node activates a series of outputs one after another.

**Type**: `TimerSequenceNode`

### Functionality
When **Enable** is True, it cycles through the configured timers. Each timer keeps its corresponding output high for its duration, then hands off to the next.

### Inputs
- **Enable** (Boolean): Starts the sequence. Turning it off resets to the first timer.

### Outputs
- **Elapsed Time** (Number): Time spent in the current timer slot (ms).
- **Timer Index** (Integer): Index of the current active timer.
- **Timer 1, Timer 2...**: Boolean outputs active during their respective slots.

### Configuration
1.  **Add Timer**: Use the Properties panel to add steps.
2.  **Duration**: Set the length and unit (ms, seconds, minutes) for each step.
3.  **Label**: Rename outputs (e.g., "Red", "Yellow", "Green") for clarity.

---

## One Shot Pulse
The **One Shot Pulse** node generates a delayed pulse.

**Type**: `OneShotPulseAfterNode`

### Functionality
On a rising edge (False -> True) at the **Trigger** input, it waits for the configured delay, then emits a single-cycle True pulse.

### Inputs
- **Trigger** (Boolean): Starts the delay timer.

### Outputs
- **Pulse** (Boolean): The delayed signal.
- **Elapsed Time** (Number): Time waited so far.

---

## Counter
The **Counter** node tracks a number based on input pulses.

**Type**: `CounterNode`

### Inputs
- **Up** (Boolean): Rising edge increments count.
- **Down** (Boolean): Rising edge decrements count.
- **Reset** (Boolean): Sets count to 0.

### Outputs
- **Value** (Integer): The current count.

### Properties
- **Initial Value**: Starting number (if available in props).
