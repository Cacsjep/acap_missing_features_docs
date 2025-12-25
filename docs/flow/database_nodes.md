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

## Best Practices

1. **Use parameters** (?) instead of string concatenation to prevent SQL injection
2. **Create indexes** for columns you frequently search on
3. **Periodically delete old data** to keep the database small
4. **Test queries** in SQL Query node before using in Insert/Update
5. **Keep one connection** and share it across multiple SQL nodes
6. **Handle errors** by connecting the Error output to a Logging node
