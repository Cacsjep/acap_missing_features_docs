# Logic & Flow Control Nodes

Nodes for boolean logic, state management, and signal routing.

---

## Logic Gates

### AND Gate

Outputs true only when all inputs are true.

**Category**: Logic

| Input A | Input B | Result |
|---------|---------|--------|
| false | false | false |
| false | true | false |
| true | false | false |
| true | true | **true** |

---

### OR Gate

Outputs true when any input is true.

**Category**: Logic

| Input A | Input B | Result |
|---------|---------|--------|
| false | false | false |
| false | true | **true** |
| true | false | **true** |
| true | true | **true** |

---

### NOT Gate

Inverts the input signal.

**Category**: Logic

| Input A | Result |
|---------|--------|
| false | **true** |
| true | false |

---

### XOR Gate (Exclusive OR)

Outputs true when inputs differ.

**Category**: Logic

| Input A | Input B | Result |
|---------|---------|--------|
| false | false | false |
| false | true | **true** |
| true | false | **true** |
| true | true | false |

---

### NAND Gate

Outputs false only when all inputs are true (inverted AND).

**Category**: Logic

| Input A | Input B | Result |
|---------|---------|--------|
| false | false | **true** |
| false | true | **true** |
| true | false | **true** |
| true | true | false |

---

### NOR Gate

Outputs true only when all inputs are false (inverted OR).

**Category**: Logic

| Input A | Input B | Result |
|---------|---------|--------|
| false | false | **true** |
| false | true | false |
| true | false | false |
| true | true | false |

---

### XNOR Gate (Exclusive NOR)

Outputs true when inputs are the same.

**Category**: Logic

| Input A | Input B | Result |
|---------|---------|--------|
| false | false | **true** |
| false | true | false |
| true | false | false |
| true | true | **true** |

---

## Comparison

### Compare

Compares two values using various operators.

**Category**: Logic, Compare

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| A | Configurable | First value |
| B | Configurable | Second value |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Result | Boolean | Comparison result |

#### Properties

- **Operator**: `==`, `!=`, `>`, `<`, `>=`, `<=`, `contains`, `startswith`, `endswith`
- **Data Type**: Integer, Float, or String

!!! note
    String comparisons support `contains`, `startswith`, and `endswith` operators.

---

### String Empty

Checks if a string is empty or whitespace-only.

**Category**: Logic, Compare

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value | String | Text to check |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Result | Boolean | True if empty |

---

## Edge Detection

### Edge

Detects rising or falling edges in a boolean signal.

**Category**: Logic, Edge

#### Functionality

Emits a single-cycle pulse when the input transitions. Configure for rising edge (false→true) or falling edge (true→false).

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value | Boolean | Signal to monitor |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Boolean | Pulses true on edge |

#### Properties

- **Mode**: Rising or Falling

---

### Impulse Storage

Buffers pulses until a release signal allows them through.

**Category**: Logic, Edge

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Add | Boolean | Rising edge stores a pulse |
| Release | Boolean | Rising edge releases one pulse |
| Clear | Boolean | Clears all stored pulses |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Pulse | Boolean | Released pulse |

#### Properties

- **Buffer Size**: Maximum stored pulses

---

### Next Cycle

Delays a pulse to the next execution cycle.

**Category**: Logic, Time

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Trigger | Boolean | Rising edge to delay |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Pulse | Boolean | Delayed pulse (next cycle) |

---

### Edge Value Mux

Routes values based on which trigger fires.

**Category**: Edge, Logic

#### Functionality

When any trigger has a rising edge, outputs a pulse and the corresponding value.

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Trigger 1-4 | Boolean | Trigger signals |
| Value 1-4 | String | Associated values |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Pulse | Boolean | One-cycle pulse |
| Value | String | Value from triggered input |

---

## State & Memory

### Flip Flop

Stores a single bit with Set/Reset control.

**Category**: Memory, Edge

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Set | Boolean | Sets state to true |
| Reset | Boolean | Sets state to false |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Boolean | Current state |

#### Properties

- **Mode**: RS (reset-dominant) or SR (set-dominant)

---

### Value Latch

Captures and holds a value until reset.

**Category**: Memory, Data

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value | Configurable | Value to capture |
| Set | Boolean | Rising edge captures value |
| Reset | Boolean | Clears stored value |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Configurable | Stored value |

---

### FIFO Queue

First-In First-Out buffer for values.

**Category**: Memory, Data

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value | Integer | Value to add |
| Add | Boolean | Rising edge enqueues |
| Pop | Boolean | Rising edge dequeues |
| Reset | Boolean | Clears queue |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Integer | Front value (oldest) |
| Has | Boolean | True if queue not empty |
| Count | Integer | Number of items |

---

### LIFO Queue (Stack)

Last-In First-Out buffer for values.

**Category**: Memory, Data

Same interface as FIFO, but `Pop` returns the most recently added value.

---

### Data Table

Multi-value register block with configurable outputs.

**Category**: Memory, Data

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| O.01-O.08 | Integer | Configurable outputs |

Configure values in the properties panel.

---

### Key-Value Store

Simple in-memory key-value storage.

**Category**: Memory

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Set | Boolean | Rising edge stores value |
| Get | Boolean | Rising edge retrieves value |
| Remove | Boolean | Rising edge deletes key |
| Key | String | Storage key |
| Value | String | Value to store |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Operation complete |
| Value | String | Retrieved value |

---

### Cache

Key-value cache with time-to-live.

**Category**: Memory

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Set | Boolean | Rising edge stores value |
| Remove | Boolean | Rising edge deletes key |
| Key | String | Cache key |
| Value | String | Value to cache |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Operation complete |
| Value | String | Cached value |

#### Properties

- **TTL**: Time-to-live in seconds

---

## Routing & Selection

### Multiplexer

Routes one of many inputs to the output based on index.

**Category**: Logic, Data

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Index | Integer | Selects input (1-based) |
| Value-1...N | Configurable | Input values |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Configurable | Selected value |
| Selected Index | Integer | Active input number |

#### Properties

- **Data Type**: Int, Float, or String
- Use Properties panel to add/remove inputs

---

### Value Selector

Binary switch between two values.

**Category**: Logic, Data

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Select | Boolean | False=Input 1, True=Input 2 |
| Input 1 | Configurable | Value when false |
| Input 2 | Configurable | Value when true |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Configurable | Selected value |

---

### Move

Passes value through when enabled.

**Category**: Data, Transform

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | True passes value |
| Value | Integer | Input value |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Integer | Output (when enabled) |

---

## Math Operations

### Bit Add

Combines boolean inputs into an integer bitfield.

**Category**: Math

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Bit 1-8 | Boolean | Individual bits |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Value | Integer | Combined bitwise value |

Bit 1 = LSB (value 1), Bit 8 = MSB (value 128)
