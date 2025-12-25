# String & Text Nodes

Nodes for text manipulation, regular expressions, and encoding operations.

---

## String Operations

### String Concat

Joins two strings with an optional separator.

**Category**: String

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String A | String | First text |
| String B | String | Second text |
| Separator | String | Text between A and B (optional) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Result | String | Combined text |

#### Example

- A: `Hello`, B: `World`, Separator: ` ` → Result: `Hello World`

---

### String Replace

Replaces all occurrences of a substring.

**Category**: String

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Original text |
| Old | String | Text to find |
| New | String | Replacement text |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Result | String | Modified text |

---

### String Split

Splits a string into parts at a separator.

**Category**: String

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Text to split |
| Separator | String | Delimiter character(s) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Parts JSON | String | JSON array of parts |

#### Example

- String: `a,b,c`, Separator: `,` → Parts JSON: `["a","b","c"]`

---

### String Join

Combines array elements into a single string.

**Category**: String

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Parts JSON | String | JSON array of strings |
| Separator | String | Text between elements |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Combined text |

---

### String Trim

Removes whitespace from both ends of text.

**Category**: String

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Text with possible whitespace |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Trimmed text |

---

### String Lower

Converts text to lowercase.

**Category**: String

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Original text |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Lowercase text |

---

### String Upper

Converts text to uppercase.

**Category**: String

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Original text |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Uppercase text |

---

### String Empty

Checks if a string is empty or contains only whitespace.

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

## Regular Expressions

### Regex Match

Tests if a pattern matches the input text.

**Category**: String, Regex

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Text to test |
| Pattern | String | Regular expression |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Match | Boolean | True if pattern matches |

#### Example Patterns

| Pattern | Matches |
|---------|---------|
| `^[A-Z]{3}` | Starts with 3 uppercase letters |
| `\d{4}` | Contains 4 consecutive digits |
| `error\|warning` | Contains "error" or "warning" |

---

### Regex Replace

Replaces text matching a pattern.

**Category**: String, Regex

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Original text |
| Pattern | String | Regular expression to match |
| Replacement | String | Replacement text |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Result | String | Modified text |

!!! tip
    Use `$1`, `$2` in the replacement to reference capture groups from the pattern.

---

### Regex Find All

Finds all matches of a pattern.

**Category**: String, Regex

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Text to search |
| Pattern | String | Regular expression |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Matches JSON | String | JSON array of all matches |

---

## Encoding

### Base64 Encode

Encodes text as Base64.

**Category**: Encoding, String

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Text to encode |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Base64 | String | Encoded text |

---

### Base64 Decode

Decodes Base64 text.

**Category**: Encoding, String

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Base64 | String | Encoded text |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Decoded text |

---

## Type Conversion

### String to Int

Parses a string as an integer.

**Category**: Transform, Data

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Numeric text (e.g., `"42"`) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Int | Integer | Parsed number (0 if invalid) |

---

### String to Float

Parses a string as a floating-point number.

**Category**: Transform, Data

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Numeric text (e.g., `"3.14"`) |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Float | Float | Parsed number (0.0 if invalid) |

---

### Int to String

Converts an integer to text.

**Category**: Transform, Data

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Int | Integer | Number to convert |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Text representation |

---

### Float to String

Converts a floating-point number to text.

**Category**: Transform, Data

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Float | Float | Number to convert |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| String | String | Text representation |

---

### Int to Bool

Converts an integer to boolean.

**Category**: Transform, Logic

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value | Integer | Number to convert |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Bool | Boolean | True if non-zero |

---

### Bool to Int

Converts a boolean to integer.

**Category**: Transform, Logic

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Value | Boolean | Boolean to convert |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Int | Integer | 1 if true, 0 if false |

---

## Data Generation

### Date String

Formats the current date and time.

**Category**: Utility, Time

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge updates outputs |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Updated | Boolean | Pulses true when updated |
| Text | String | Formatted date/time |
| Unix (s) | Integer | Unix timestamp in seconds |
| Unix (ms) | Integer | Unix timestamp in milliseconds |

#### Properties

- **Format**: Date/time format string (e.g., `2006-01-02 15:04:05`)

!!! note
    Uses Go time format: `2006` = year, `01` = month, `02` = day, `15` = hour, `04` = minute, `05` = second.

---

### Random Number

Generates a random integer or float within a range.

**Category**: Utility

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge generates new value |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Updated | Boolean | Pulses true when generated |
| Value | Int/Float | Random number |

#### Properties

- **Type**: Integer or Float
- **Min**: Minimum value (inclusive)
- **Max**: Maximum value (inclusive)

---

### CSV Row Builder

Constructs a CSV row from dynamic inputs.

**Category**: Output, Transform

#### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge builds the row |
| Column inputs | String | One input per column |

#### Outputs

| Name | Type | Description |
|------|------|-------------|
| Built | Boolean | Pulses true when complete |
| Row | String | CSV-formatted row |

#### Properties

- **Columns**: Define column names; each becomes an input
- **Separator**: Field separator (default: `,`)
- **Quote**: Quote character for text fields

!!! tip
    Combine with SD Card Append File to log data to CSV files.
