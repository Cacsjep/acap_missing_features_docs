# Database Nodes

Nodes for connecting to SQL databases and executing queries. Supports SQLite, MySQL, PostgreSQL, and SQL Server.

---

## Overview

The database nodes allow you to store and retrieve data from SQL databases directly within your Flow. This is useful for:

- Logging events with timestamps
- Storing sensor readings over time
- Managing device configurations
- Building reports from historical data

---

## SQLite Quick Start

SQLite is the easiest database to use because it stores everything in a single file on the SD card - no server needed.

### Complete Example: Event Logging

This example creates a database, table, and logs events with timestamps.

#### Step 1: Create the Database and Table

First, create the SQLite database file and table structure. You only need to do this once.

**Using SD Card Write File node:**

Create a file called `init.sql` on the SD card with this content:

```sql
CREATE TABLE IF NOT EXISTS events (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp TEXT DEFAULT CURRENT_TIMESTAMP,
    event_type TEXT NOT NULL,
    source TEXT,
    value REAL
);
```

Then use SQL Connection + SQL Query to execute this:

1. Add **SQL Connection** node
   - Driver: `sqlite`
   - Path: `events.db` (stored on SD card)
   - Connect the `Connect` input to an Enable node

2. Add **SQL Query** node
   - Connect `Conn Key` from the SQL Connection
   - SQL: `CREATE TABLE IF NOT EXISTS events (id INTEGER PRIMARY KEY AUTOINCREMENT, timestamp TEXT DEFAULT CURRENT_TIMESTAMP, event_type TEXT NOT NULL, source TEXT, value REAL)`
   - Trigger `Execute` once to create the table

#### Step 2: Insert Events

Now log events when they occur:

1. Add **SQL Insert** node
   - Connect `Conn Key` from SQL Connection
   - SQL: `INSERT INTO events (event_type, source, value) VALUES (?, ?, ?)`
   - Params JSON: Use Placeholder JSON or build manually

**Example Flow for Motion Detection Logging:**

```
[Metadata Event Subscribe] → [Edge Detection] → [SQL Insert]
        ↓                                              ↓
   "motion" event                              Logs to database
```

**Params JSON format:**
```json
["motion_detected", "Camera-1", 1.0]
```

#### Step 3: Query Data

Read back the logged events:

1. Add **SQL Query** node
   - SQL: `SELECT * FROM events ORDER BY timestamp DESC LIMIT 10`
   - Connect output to JSON Viewer or Table widget

**Output format:**
```json
[
  {"id": 3, "timestamp": "2025-12-25 10:30:45", "event_type": "motion_detected", "source": "Camera-1", "value": 1.0},
  {"id": 2, "timestamp": "2025-12-25 10:28:12", "event_type": "door_open", "source": "Entrance", "value": null}
]
```

---

## SQL Connection

Establishes a connection to a database. Other SQL nodes use the connection key from this node.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Connect | Boolean | Rising edge opens connection, falling edge closes |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Connected | Boolean | True while connection is active |
| Conn Key | String | Pass this to other SQL nodes |
| Error | String | Connection error message |

### Properties

| Property | Description |
|----------|-------------|
| Driver | Database type: `sqlite`, `mysql`, `postgres`, `sqlserver` |
| Host | Server address (not used for SQLite) |
| Port | Server port (not used for SQLite) |
| Database | Database name (not used for SQLite) |
| Username | Login username (not used for SQLite) |
| Password | Login password (not used for SQLite) |
| Path | SQLite file path on SD card (e.g., `mydata.db`) |

### Driver-Specific Configuration

#### SQLite
```
Driver: sqlite
Path: mydata.db
```
The file is created on the camera's SD card. No host/port/credentials needed.

#### MySQL
```
Driver: mysql
Host: 192.168.1.100
Port: 3306
Database: myapp
Username: user
Password: ****
```

#### PostgreSQL
```
Driver: postgres
Host: 192.168.1.100
Port: 5432
Database: myapp
Username: user
Password: ****
```

#### SQL Server
```
Driver: sqlserver
Host: 192.168.1.100
Port: 1433
Database: myapp
Username: user
Password: ****
```

!!! tip
    Keep one SQL Connection node and wire its `Conn Key` to multiple query/insert nodes. This shares the connection efficiently.

---

## SQL Query

Executes a SELECT statement and returns results as JSON.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge runs the query |
| Conn Key | String | Connection from SQL Connection node |
| SQL | String | The SELECT statement |
| Params JSON | String | Optional JSON array of parameter values |
| Reset | Boolean | Clears cached results |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when query completes |
| Rows JSON | String | JSON array of result rows |
| Row Count | Integer | Number of rows returned |
| Error | String | Error message if query fails |

### Examples

**Simple query:**
```sql
SELECT * FROM events WHERE event_type = 'motion'
```

**Query with parameters (safer, prevents SQL injection):**
```sql
SELECT * FROM events WHERE source = ? AND value > ?
```
Params JSON: `["Camera-1", 0.5]`

**Query with date filter:**
```sql
SELECT * FROM events WHERE timestamp > datetime('now', '-1 hour')
```

**Aggregate query:**
```sql
SELECT event_type, COUNT(*) as count FROM events GROUP BY event_type
```

---

## SQL Insert

Inserts new rows into a table.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge runs the insert |
| Conn Key | String | Connection from SQL Connection node |
| SQL | String | The INSERT statement |
| Params JSON | String | JSON array of values to insert |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when insert completes |
| Rows Affected | Integer | Number of rows inserted (usually 1) |
| Last Insert ID | Integer | Auto-generated ID of inserted row |
| Error | String | Error message if insert fails |

### Examples

**Insert with parameters:**
```sql
INSERT INTO events (event_type, source, value) VALUES (?, ?, ?)
```
Params JSON: `["temperature", "Sensor-1", 23.5]`

**Insert with current timestamp (SQLite):**
```sql
INSERT INTO events (event_type, source) VALUES (?, ?)
```
The `timestamp` column uses `DEFAULT CURRENT_TIMESTAMP` automatically.

### Building Params JSON Dynamically

Use **Placeholder JSON** node to build the parameters:

Template:
```json
["$event_type", "$source", $value]
```

This creates inputs for `event_type`, `source`, and `value` that you can wire from other nodes.

---

## SQL Update

Updates existing rows in a table.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge runs the update |
| Conn Key | String | Connection from SQL Connection node |
| SQL | String | The UPDATE statement |
| Params JSON | String | JSON array of parameter values |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when update completes |
| Rows Affected | Integer | Number of rows modified |
| Error | String | Error message if update fails |

### Examples

**Update single row:**
```sql
UPDATE devices SET last_seen = CURRENT_TIMESTAMP WHERE device_id = ?
```
Params JSON: `["Camera-1"]`

**Update with multiple conditions:**
```sql
UPDATE events SET processed = 1 WHERE event_type = ? AND processed = 0
```
Params JSON: `["motion"]`

---

## SQL Delete

Deletes rows from a table.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Execute | Boolean | Rising edge runs the delete |
| Conn Key | String | Connection from SQL Connection node |
| SQL | String | The DELETE statement |
| Params JSON | String | JSON array of parameter values |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Done | Boolean | Pulses true when delete completes |
| Rows Affected | Integer | Number of rows deleted |
| Error | String | Error message if delete fails |

### Examples

**Delete old records:**
```sql
DELETE FROM events WHERE timestamp < datetime('now', '-30 days')
```

**Delete by ID:**
```sql
DELETE FROM events WHERE id = ?
```
Params JSON: `[42]`

!!! warning
    Always use a WHERE clause to avoid deleting all rows accidentally.

---

## SQL Table

Create and insert data into a database table without writing SQL. Define your table structure visually with columns, and the node automatically creates the table and handles inserts.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from SQL Connection node |
| Insert | Boolean | Rising edge inserts a new row with values from column inputs |
| Clear | Boolean | Rising edge deletes all rows from the table |
| *Column Inputs* | Various | One input per column you define (created via Sync Inputs) |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Success | Boolean | True when last operation succeeded |
| Last ID | Integer | Auto-generated ID of last inserted row |
| Row Count | Integer | Total number of rows in the table |

### Properties

| Property | Description |
|----------|-------------|
| Table Name | Name of the database table |
| Include ID Column | Add auto-incrementing primary key (recommended) |
| Include Timestamp | Add automatic `datetime` column with current time |
| Columns | Define columns with name, data type, default value, and max length |

### Column Configuration

Click **Add Column** to define table columns:

- **Column Name**: Use lowercase with underscores (e.g., `sensor_value`)
- **Data Type**: Boolean, Integer, Float, or String
- **Default Value**: Optional SQL default
- **Max Length**: For strings, 0 = unlimited (TEXT), or specify max chars (VARCHAR)

Click **Sync Inputs** after defining columns to create the corresponding node inputs.

### How It Works

1. **Auto-Create Table**: The table is automatically created if it doesn't exist
2. **Auto-Migrate**: New columns are automatically added when you update the schema
3. **Insert on Trigger**: Send a rising edge to Insert to save a row with current input values
4. **Clear on Trigger**: Send a rising edge to Clear to delete all rows

### Example: Event Logger

**Properties:**
- Table Name: `events`
- Include ID: Yes
- Include Timestamp: Yes
- Columns:
    - `event_type` (String)
    - `source` (String)
    - `value` (Float)

**Generated SQL:**
```sql
CREATE TABLE IF NOT EXISTS events (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  event_type TEXT,
  source TEXT,
  value REAL,
  datetime DATETIME DEFAULT (datetime('now', 'localtime'))
);
```

Wire your data sources to the column inputs, then trigger Insert to log a row.

---

## SQL Table Read

Query a database table and output rows as JSON. Configure the query visually without writing SQL.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from SQL Connection node |
| Read | Boolean | Rising edge executes the query and updates outputs |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Success | Boolean | True when query succeeded |
| Rows JSON | String | Query results as JSON array of objects |
| Row Count | Integer | Number of rows returned |

### Properties

| Property | Description |
|----------|-------------|
| Table Name | Name of the table to query |
| Columns | Columns to select (`*` for all, or comma-separated) |
| Order By | Column to sort by |
| Order Direction | Descending (newest first) or Ascending |
| Limit | Maximum rows to return |
| Offset | Skip this many rows (for pagination) |
| Filters | WHERE conditions to filter rows |

### Filters

Add filters to select specific rows. Each filter has:

- **Column**: The column to filter on
- **Value Type**:
    - **Static Value**: Enter a fixed value with comparison operator (=, !=, >, <, LIKE, etc.)
    - **Relative Time**: Filter by time range (last 15 minutes, 1 hour, 7 days, etc.)

When using **Relative Time**, the operator is automatically set to `>` (greater than) to get recent records.

### Time Filter Presets

| Preset | Description |
|--------|-------------|
| 1m - 45m | Minutes (1, 2, 5, 10, 15, 20, 30, 45) |
| 1h - 24h | Hours (1, 2, 3, 4, 6, 8, 12, 18, 24) |
| 2d - 180d | Days (2, 3, 5, 7, 14, 21, 30, 60, 90, 180) |
| 1y | One year |

### Example: Recent Events

**Properties:**
- Table Name: `events`
- Columns: `*`
- Order By: `datetime`
- Order Direction: Descending
- Limit: 100
- Filter: `datetime` > Last 15 minutes

**Generated SQL:**
```sql
SELECT * FROM events
WHERE datetime > datetime('now', 'localtime', '-15 minutes')
ORDER BY datetime DESC
LIMIT 100;
```

### Example: Filtered Query

**Properties:**
- Table Name: `temperatures`
- Columns: `datetime, temperature`
- Filter 1: `sensor_id` = `Sensor-1` (Static Value)
- Filter 2: `datetime` > Last 1 hour (Relative Time)
- Order By: `datetime`
- Limit: 50

**Generated SQL:**
```sql
SELECT datetime, temperature FROM temperatures
WHERE sensor_id = 'Sensor-1'
  AND datetime > datetime('now', 'localtime', '-1 hours')
ORDER BY datetime DESC
LIMIT 50;
```

!!! tip
    Use SQL Table and SQL Table Read together: SQL Table to log data, SQL Table Read to query it back for display in Dashboard widgets.

---

## Complete Example: Temperature Logger

This example logs temperature readings from a Modbus sensor and displays recent values.

### Database Schema

```sql
CREATE TABLE IF NOT EXISTS temperatures (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp TEXT DEFAULT CURRENT_TIMESTAMP,
    sensor_id TEXT NOT NULL,
    temperature REAL NOT NULL
);
```

### Flow Structure

```
[Modbus TCP Client] ─→ [Modbus Read Float] ─→ [SQL Insert]
        │                      │                    │
        │                      ↓                    │
        │              temperature value            │
        │                                           │
        └──────────────────────────────────────────→ (Conn Key)
```

### Node Configuration

**1. SQL Connection**
- Driver: `sqlite`
- Path: `temperatures.db`

**2. SQL Query (create table - run once)**
- SQL: `CREATE TABLE IF NOT EXISTS temperatures (id INTEGER PRIMARY KEY AUTOINCREMENT, timestamp TEXT DEFAULT CURRENT_TIMESTAMP, sensor_id TEXT NOT NULL, temperature REAL NOT NULL)`

**3. Pulse Generator**
- Period: 60 seconds (log every minute)

**4. Modbus Read Float**
- Address: 100 (your sensor register)

**5. Placeholder JSON**
- Template: `["Sensor-1", $temperature]`
- Wire temperature from Modbus Read Float

**6. SQL Insert**
- SQL: `INSERT INTO temperatures (sensor_id, temperature) VALUES (?, ?)`
- Params JSON: from Placeholder JSON

**7. SQL Query (for display)**
- SQL: `SELECT timestamp, temperature FROM temperatures ORDER BY id DESC LIMIT 100`
- Wire to Dashboard Chart widget

---

## Common SQL Patterns

### Get Latest Value
```sql
SELECT * FROM readings ORDER BY id DESC LIMIT 1
```

### Count Records Today
```sql
SELECT COUNT(*) as count FROM events
WHERE date(timestamp) = date('now')
```

### Average Over Time Period
```sql
SELECT AVG(value) as average FROM readings
WHERE timestamp > datetime('now', '-1 hour')
```

### Find Min/Max Values
```sql
SELECT MIN(temperature) as min_temp, MAX(temperature) as max_temp
FROM temperatures WHERE date(timestamp) = date('now')
```

### Cleanup Old Data
```sql
DELETE FROM events WHERE timestamp < datetime('now', '-7 days')
```

Run this periodically with a Timer to prevent the database from growing too large.

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "database is locked" | Only one connection can write at a time. Check for parallel inserts. |
| "no such table" | Run the CREATE TABLE query first |
| "SQLITE_READONLY" | SD card might be full or write-protected |
| Connection fails | For remote databases, check network/firewall settings |
| Empty results | Verify table name and column names match exactly |
| Wrong data types | Ensure Params JSON values match expected types |

---

## Parking Monitor

The **Parking Monitor** node tracks vehicle entry/exit times, calculates parking duration, and detects overparking. Uses SQL storage for persistence.

**Category**: Database, Plates

### Functionality

Manages a parking database with automatic overparking detection and cleanup. Ideal for parking lot monitoring systems.

### Inputs

| Name | Type | Description |
|------|------|-------------|
| Conn Key | String | Connection from SQL Connection node |
| Plate | String | License plate text |
| Tags | String | Optional tags for the entry (e.g., zone, vehicle type) |
| Enter | Boolean | Rising edge records vehicle entry |
| Exit | Boolean | Rising edge records vehicle exit |
| Check Overstay | Boolean | Rising edge checks for overparked vehicles |
| Cleanup | Boolean | Rising edge deletes old entries |
| Delete | Boolean | Rising edge deletes specific plate |
| Delete Plate | String | Plate to delete when Delete triggers |
| Max Park Mins | Integer | Max parking minutes before overparked |
| Max Hold Mins | Integer | Delete entries older than this on cleanup |

### Outputs

| Name | Type | Description |
|------|------|-------------|
| Overparked | Boolean | Pulses true when overparked plate found |
| Overprk Plate | String | The overparked license plate |
| Overprk Mins | Integer | Minutes parked for overparked plate |
| Entries JSON | String | All entries sorted by minutes (longest first) |

### Properties

| Property | Description |
|----------|-------------|
| Table Name | Database table name (default: `parking_entries`) |
| Auto Check Overstay | Enable automatic overstay checking |
| Check Interval | Seconds between auto checks (default: 10) |
| Auto Cleanup | Enable automatic cleanup of old entries |
| Cleanup Interval | Seconds between auto cleanups (default: 3600) |
| Max Park Minutes | Default max parking time (default: 120) |
| Max Hold Minutes | Default max entry age for cleanup (default: 720) |

### Entries JSON Format

```json
[
  {
    "plate": "ABC123",
    "tags": "Zone A",
    "entry_time": 1704444000,
    "entry_time_human": "2024-01-05 08:12:00",
    "minutes": 125,
    "seconds": 7512,
    "is_overparked": true,
    "is_exit": false,
    "exit_time": 0,
    "exit_time_human": ""
  }
]
```

### Usage Example

1. Add **SQL Connection** node (SQLite or other database)
2. Add **Parking Monitor** node and connect `Conn Key`
3. Wire LPR events (from Metadata Event Subscribe or Vaxtor) to `Plate`, `Enter`, and `Exit`
4. Enable auto check overstay in properties, or wire a Timer to `Check Overstay`
5. Wire `Overparked` output to trigger alerts

!!! tip
    Use the `Tags` input to categorize entries by zone, camera, or vehicle type. This data is included in the Entries JSON.

---

## Best Practices

1. **Use parameters** (?) instead of string concatenation to prevent SQL injection
2. **Create indexes** for columns you frequently search on
3. **Periodically delete old data** to keep the database small
4. **Test queries** in SQL Query node before using in Insert/Update
5. **Keep one connection** and share it across multiple SQL nodes
6. **Handle errors** by connecting the Error output to a Logging node
