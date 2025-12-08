# Logic & Flow Control Nodes

Nodes for boolean logic, state management, and signal routing.

## Logic Gate
The **Logic Gate** node performs standard boolean operations. The type is determined by the specific node added (AND, OR, NOT, etc.).

**Type**: `LogicNode`

### Behavior
- **AND**: True if both A and B are True.
- **OR**: True if A or B is True.
- **XOR**: True if exactly one input is True.
- **NOT**: Inverts input A.

### Inputs
- **A** (Boolean)
- **B** (Boolean) - Ignored for NOT.

---

## Flip Flop
Stores a binary state (Set/Reset).

**Type**: `FlipFlopNode`

### Inputs
- **Set**: Sets state to True.
- **Reset**: Sets state to False.
- **Toggle**: Inverts state.

### Outputs
- **State**: Current state.
- **Inverse**: Inverse of state.

---

## Multiplexer
Routes one of multiple input values to the output based on an index.

**Type**: `MultiplexerNode`

### Functionality
Acts like a switch. If **Index** is 1, it passes Input 1. If **Index** is 2, it passes Input 2, etc.

### Inputs
- **Index** (Integer): Selects which input to pass (1-based index).
- **I.01, I.02...**: Value inputs.

### Outputs
- **Output**: The selected input value.

### Properties
- **Add/Remove Input**: Use the buttons in the properties panel to change the number of inputs.
- **Data Type**: Select the type for all inputs/outputs (Int, Float, String, etc.).

---

## Value Selector
Selects between two values based on a boolean condition.

**Type**: `ValueSelectorNode`

### Functionality
If **Select** is True, passes **Input 1**. If False, passes **Input 0**.

### Inputs
- **Select** (Boolean): The control signal.
- **In 0** (Any): Value when Select is False.
- **In 1** (Any): Value when Select is True.

### Outputs
- **Output**: The selected value.

### Properties
- **Data Type**: Configure the type of values being switched.
