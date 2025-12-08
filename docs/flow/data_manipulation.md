# Data Manipulation Nodes

Nodes for arithmetic, text processing, and data conversion.

## Math
The **Math** node performs basic arithmetic operations.

**Category**: Data
**Type**: `MathNode`

### Functionality
Performs an operation on two inputs.

### Inputs
- **In 1** (Number): First operand.
- **In 2** (Number): Second operand.

### Outputs
- **Result** (Number): The result of the operation.

### Properties
- **Operation**:
  - **Add**: `In 1 + In 2`
  - **Subtract**: `In 1 - In 2`
  - **Multiply**: `In 1 * In 2`
  - **Divide**: `In 1 / In 2`

---

## Math Advanced
The **Math Advanced** node performs complex mathematical functions.

**Category**: Data
**Type**: `MathAdvancedNode`

### Inputs
- **In** (Number): The input value.

### Outputs
- **Result** (Number): The calculated result.

### Properties
- **Operation**:
  - **ABS**: Absolute value.
  - **SQRT**: Square root.
  - **SIN/COS/TAN**: Trigonometric functions (radians).
  - **SIGN**: Returns -1, 0, or 1.

---

## Text
The **Text** node outputs a static or dynamic text string, often used for labeling or debugging on the canvas.

**Category**: Data
**Type**: `TextNode`

### Functionality
Displays text on the flow canvas. Can be used as a comment or a constant string source.

### Inputs
- **Input** (String/Any): Dynamic text to display. If connected, overrides the static text property.

### Outputs
- **Output** (String): The current text.

### Properties
- **Text**: Static text content.
- **Size**: Font size.
- **Color**: Text color (Hex code).

---

## JSON Key Extract
The **JSON Key Extract** node extracts a value from a JSON string or object.

**Category**: Data
**Type**: `JsonKeyExtractNode`

### Functionality
Parses the input JSON and outputs the value at the specified key.

### Inputs
- **JSON** (String/Object): The JSON data.

### Outputs
- **Value** (Any): The extracted value.

### Properties
- **Key**: The key path to extract (e.g., `data.temperature`).
