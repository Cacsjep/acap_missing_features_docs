# Collections & JSON Nodes

Nodes for working with lists, maps, sets, and JSON data structures.

---

## List Operations

Lists are JSON arrays that can store ordered collections of values. These nodes manipulate lists without requiring Lua scripting.

### List Length

Returns the number of elements in a JSON list.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| List | String | JSON array (e.g., `["a","b","c"]`) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Length | Integer | Number of elements |

---

### List Get At

Retrieves the element at a specific index.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| List | String | JSON array |
| Index | Integer | Zero-based index |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| Item | String | The element at the index (JSON encoded) |

---

### List Set At

Replaces the element at a specific index.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| List | String | JSON array |
| Index | Integer | Zero-based index |
| Item | String | New value (JSON encoded) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| List | String | Updated JSON array |

---

### List Push

Appends an item to the end of a list.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| List | String | JSON array |
| Item | String | Value to append |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| List | String | Updated JSON array |
| New Length | Integer | Length after push |

---

### List Pop

Removes and returns the last item from a list.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| List | String | JSON array |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| List | String | Updated JSON array |
| Popped Item | String | The removed element |

---

### List Insert At

Inserts an item at a specific index, shifting subsequent elements.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| List | String | JSON array |
| Index | Integer | Position to insert at |
| Item | String | Value to insert |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| List | String | Updated JSON array |

---

### List Remove At

Removes and returns the item at a specific index.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| List | String | JSON array |
| Index | Integer | Position to remove |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| List | String | Updated JSON array |
| Removed Item | String | The removed element |

---

### List Contains

Checks if a list contains a specific value.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| List | String | JSON array |
| Item | String | Value to search for |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Contains | Boolean | True if found |

---

### List Index Of

Finds the index of a value in a list.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| List | String | JSON array |
| Item | String | Value to find |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| Index | Integer | Position of item (-1 if not found) |

---

### List For Each

Iterates over a list, emitting each item one at a time.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Start | Boolean | Rising edge begins iteration |
| List | String | JSON array to iterate |
| Reset | Boolean | Stops iteration and resets |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Index | Integer | Current position |
| Item | String | Current element |
| Has Next | Boolean | True if more items remain |
| Done | Boolean | Pulses true when iteration completes |

!!! tip
    Use List For Each with a Pulse Generator to process items at a controlled rate.

---

## Map Operations

Maps are JSON objects that store key-value pairs. Use these nodes to manipulate object properties.

### Map Get

Retrieves a value by key from a JSON object.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| Map | String | JSON object |
| Key | String | Property name |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| Value | String | The value (JSON encoded) |

---

### Map Set

Sets or updates a key-value pair in a JSON object.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| Map | String | JSON object |
| Key | String | Property name |
| Value | String | New value |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| Map | String | Updated JSON object |

---

### Map Remove

Removes a key from a JSON object.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| Map | String | JSON object |
| Key | String | Property to remove |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| Map | String | Updated JSON object |

---

### Map Contains Key

Checks if a key exists in a JSON object.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Map | String | JSON object |
| Key | String | Property name |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Contains | Boolean | True if key exists |

---

### Map Keys

Lists all keys from a JSON object.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Map | String | JSON object |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Keys | String | JSON array of key names |

---

### Map Values

Lists all values from a JSON object.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Map | String | JSON object |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Values | String | JSON array of values |

---

### Map Merge

Combines two JSON objects. Values from Map B override Map A on conflicts.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| Map A | String | First JSON object |
| Map B | String | Second JSON object (takes priority) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| Map | String | Merged JSON object |

---

## Set Operations

Sets are JSON arrays that contain only unique values.

### Set Add

Adds a unique item to a set (no duplicates).

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| Set | String | JSON array (set) |
| Item | String | Value to add |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| Set | String | Updated set |

---

### Set Remove

Removes an item from a set.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| Set | String | JSON array (set) |
| Item | String | Value to remove |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| Set | String | Updated set |

---

### Set Contains

Checks if a set contains a value.

**Category**: Collections, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Set | String | JSON array (set) |
| Item | String | Value to check |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Contains | Boolean | True if item exists |

---

## JSON Utilities

### JSON Parse

Validates and normalizes JSON text.

**Category**: JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| JSON | String | Raw JSON text |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| JSON | String | Parsed and normalized JSON |
| Valid | Boolean | True if parsing succeeded |

---

### JSON Get

Extracts a value at a dotted path (e.g., `user.address.city`).

**Category**: JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers extraction |
| JSON | String | JSON text |
| Path | String | Dot notation path |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| Value | String | Extracted value |

---

### JSON Set

Sets a value at a dotted path.

**Category**: JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| JSON | String | JSON text |
| Path | String | Dot notation path |
| Value | String | New value |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| JSON | String | Updated JSON |

---

### JSON Remove

Removes a value at a dotted path.

**Category**: JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge triggers the operation |
| JSON | String | JSON text |
| Path | String | Dot notation path |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when complete |
| JSON | String | Updated JSON |

---

### JSON Validate

Checks if text is valid JSON.

**Category**: JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| JSON | String | Text to validate |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Valid | Boolean | True if valid JSON |

---

### Placeholder JSON

Renders a JSON template with placeholder substitution.

**Category**: Transform, JSON

#### Functionality

Define a JSON template with `$placeholders` in the properties. Each placeholder becomes an input. When triggered, the node replaces placeholders with input values.

#### Example

Template:
```json
{"device": "$device_id", "value": $sensor_value}
```

Creates inputs: `device_id` (String), `sensor_value` (dynamic)

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| JSON | String | Rendered JSON with substituted values |

---

### JSON List

A persistent list that stores a JSON array between executions.

**Category**: Memory, Data, JSON

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Store | Boolean | Rising edge stores the input list |
| JSON List | String | JSON array to store |
| Pop First | Boolean | Rising edge removes first item |
| Pop Last | Boolean | Rising edge removes last item |
| Clear | Boolean | Rising edge empties the list |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Count | Integer | Number of stored items |

!!! tip
    Use JSON List to accumulate events over time, then process them with List For Each.
