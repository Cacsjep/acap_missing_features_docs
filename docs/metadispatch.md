# InfluxDB Push Configuration


The MetaDispatch feature acts as a unified gateway for forwarding metadata and metrics to various external services. It supports multiple sink types:

- **InfluxDB** (Time-series database)
- **SQL Databases** (via MySQL, PostgreSQL, SQL Server)
- **Webserver** (RESTful interfaces)


---

### Common Data Fields

Regardless of the connection type, each data record includes the following elements:

- **Measurement Name**  
  A string identifier for the metric/metadata (e.g., `"cpu_usage"`, `"disk_usage"`, `"metadata"`).

- **Tags**  
  A key-value map (string pairs) providing contextual metadata.  
  _Example_:  
  ```json
  { "mac": "00:11:22:33:44:55", "device": "sda1" }
  ```
- **Fields**:
  A key-value map containing the actual metric values or metadata information. Field types include numbers, booleans, and strings.
  
    _Example_:  

    ```json
    { "usage_percent": 75.5, "total": 1000000000 }
    ```
- **Timestamp**  
  Indicating when the data was recorded.


### InfluxDB

- **Data Format Overview**:  
  Data is sent to InfluxDB as *points* that comprise a measurement name, tags, fields, and a timestamp.
    - **Example Point (Line Protocol)**:
      cpu_usage,mac=00:11:22:33:44:55 usage_percent=75.5 1640995200000000000

- **Implementation Details**:
    - **Measurement**: Directly corresponds to the metric name.
    - **Tags**: Provided as a JSON object that InfluxDB converts into tags.
    - **Fields**: Key-value pairs are stored as InfluxDB fields.
    - **Timestamp**: A `time.Time` value from Go is used as the measurement’s timestamp.

---

### Rational Databases 

#### Schema
Data is stored in a relational table **metric_records** using a schema similar to the following.


!!! Note
    The `metric_records` table is automatically created on first connection.

- **Example Table Schema** (MySQL/PostgreSQL/SQL Server):
```sql
CREATE TABLE metric_records (
    id          SERIAL PRIMARY KEY,
    measurement VARCHAR(255) NOT NULL,
    tags        JSON,
    fields      JSON,
    timestamp   TIMESTAMP NOT NULL
);
```

- **Serialization Details**:
    - Tags and Fields: Serialized to JSON strings.
    - Timestamp: Stored in an indexed datetime column.
    - Measurement: The name of the metric is stored in the measurement column.

#### DSN 

A **Data Source Name (DSN)** is a connection string that provides the necessary details for your application to establish a connection with a database. It typically comprises the following components:

- **Username and Password:** Authentication credentials for accessing the database.
- **Host and Port:** The address and port number where the database server is located.
- **Database Name:** The specific database instance to which you want to connect.
- **Additional Parameters:** Optional settings such as character set, SSL mode, timezone, and connection timeout that can fine-tune the connection behavior.


#### Best Practices for DSN Usage

- **Customization:**  
  Include all necessary parameters to match your deployment environment, such as SSL settings, time zone configurations, and connection timeout values.

- **Error Handling:**  
  Validate the DSN string carefully. If the connection fails, review the DSN for typos, missing parameters, or incorrect values.

- **Reference Documentation:**  
  Consult the official documentation of your database driver for a comprehensive list of supported DSN parameters and recommended practices.

### Webserver

- **Data Format Overview**:  
  Data is transmitted as a JSON payload via an HTTP POST request.

```json
{
   "fields":{
      "active":false
   },
   "measurement":"metadata",
   "tags":{
      "context":"Virtual input",
      "mac":"B8A44F631339",
      "name":"device_io_virtualinput",
      "topic":"tns1:Device/tnsaxis:IO/VirtualInput"
   },
   "timestamp":1744780731
}
```


#### Additional HTTP Settings

##### Authentication & Headers

- **Authentication Mode:**  
    Select the mode that best suits your web server:
    - **None:** No authentication.
    - **Basic:** Basic authentication as specified in RFC 7617.
    - **Digest:** Digest authentication as defined in RFC 7616.
    - **Bearer:** Bearer token authentication as outlined in RFC 6750.
    - **Plain:** Sends the username and password in the JSON payload (non-standard; use with caution).

- **HTTP Headers:**  
    Optionally add one or more HTTP headers to include in the POST request.
  
##### SSL/TLS Options:

HTTPS mode settings, such as skipping SSL verification, can be configured as needed.


### Datapoints

This feature introduces two distinct approaches for data collection:

- **Non-Metadata Data Points:**  
  All non-metadata data points have an interval-based configuration. You can specify how frequently each datapoint/metric is collected.

    !!! Note
        Each non-metadata data point can be added only once.

- **Metadata Data Points:**  
  Metadata data points are event-based. They are not collected on a fixed interval but are triggered by specific events.

    !!! Tip
        You can add as many metadata events as you want, allowing you to capture detailed information exactly when it matters.

---

#### Data Point Buttons

**Edit Data Point**

![](images/edit.PNG)

**Remove Data Point**

![](images/delete.PNG)

#### Configure Non-Metadata Data Points


##### Add Non-Metadata Data Points

!!! tip "Examples of Non-Metadata Data Points"
    - CPU
    - Disk
    - Uptime
    - etc.

Click the `ADD DATAPOINT` button to open the data point adding menu.
![](images/add.PNG)

Select the data point to add.
![](images/menu.PNG)

!!! warning "Note"
    If any data point is grayed out (disabled), it has already been added and cannot be added twice.


##### Configure Non-Metadata Data Points
To configure the data point, click on the `Edit Data Point` button.

- **Set Interval:**  
  Define the desired interval at which data should be pushed to InfluxDB.

Click the `DONE` button when you have finished, then click the `SAVE` button.


#### Configure Metadata Data Points

##### Add Metadata Data Points
Click the `ADD DATAPOINT` button to open the data point adding menu and select `Metadata`.

![](images/add.PNG)

![](images/menu.PNG)

After adding a new metadata datapoint, you will see that the configuration is initially incomplete. 

[![](images/notconf.PNG)](images/notconf.PNG)

##### Configure Metadata Data Points

Open the configuration via the Edit button.

![](images/edit.PNG)

- **Data Point Name:**  
  For better differentiation in the configuration list, you can change the name of the data point.
- **Select Trigger:**  
  Choose the event that will trigger the metadata capture (e.g., sensor threshold exceeded, error event, etc.).
- **Define Metadata Fields:**  
  Specify which metadata fields should be included in the payload.
- **Define InfluxDB Data Point Name:**  
  The InfluxDB data point name is the key with which the data is stored in InfluxDB.
- **Repeat:**  
  Add as many metadata events as needed.
- **Save Configuration:**  
  After finishing the configuration, don't forget to click the `SAVE` button.


##### Metadata Configuration Example

**Metadata Day Night**  
[![](images/dn.PNG)](images/dn.PNG)

**Metadata AXIS Object Analytics - Area Count Humans Example**  
Push the Day Night metadata event  
[![](images/human.PNG)](images/human.PNG)

The final configuration would look like this:  
[![](images/both.PNG)](images/both.PNG)


#### Grafana and InfluxDB Integration

This guide shows you how to integrate Grafana with InfluxDB using the **Flux** query language. 
Follow the steps below to set up your datasource and create a dashboard for visualizing data/metrics from your AXIS Camera.

---

##### Prerequisites

!!! info 
    You should already have everything in place, as it’s configured identically to the Missing Feature ACAP setup.

- **Missing Feature ACAP:** InfluxDB Push enabled, and configured.
- **InfluxDB:** A running InfluxDB instance. 
- **InfluxDB API Token:** ACAP uses token-based authentication with InfluxDB.
- **InfluxDB Bucket:** You need an InfluxDB bucket.
- **InfluxDB Org:** You need the name of your InfluxDB organization.

---

##### Add InfluxDB Datasource in Grafana

First, navigate to your Grafana instance and add a new InfluxDB datasource.

![New Datasource](images/new_source.PNG)

---

##### Configure Datasource Settings

- **Datasource Name:** Set a name for your datasource.
- **Query Language:** Select **Flux** as the query language.

![Select Flux](images/flux.PNG)

---

##### Set Your InfluxDB HTTP URL

Specify your InfluxDB HTTP URL in the settings.

![InfluxDB Endpoint](images/endpoint.PNG)

---

##### Enter InfluxDB Credentials

Fill in your InfluxDB Organization, Token, and Default Bucket.

![InfluxDB Credentials](images/ifdb.PNG)

---

##### Create a Dashboard

Navigate to the dashboard section in Grafana and add a new visualization. For example, create a panel to monitor the **Missing Features CPU usage**.

[![](images/example_cpu_mf.PNG)](images/example_cpu_mf.PNG)

---

##### Extracting Flux Scripts from InfluxDB Data Explorer for Grafana

Use the InfluxDB Data Explorer to inspect the data and use the query builder in combinitation, to switch afterwards to script editor mode to copy the flux script for grafana.
[![](images/explore.PNG)](images/explore.PNG)

For example, let's locate our metadata event **device_io_virtualport** that we previously configured and executed to ensure an entry exists in the database:

- Select the bucket.
- Choose `metadata`.
- Set the filter to `name`.
- Select `device_io_virtualport`.
- Uncheck the `AGGREATE Function` on the right side (it's a boolean value).
- Enable the `View Raw Data` mode to display our entries in a table.


[![](images/io_metadata.PNG)](images/io_metadata.PNG)

- Click on the **Script Editor** button to view the Flux script.
- You can now use this Flux script in Grafana.

[![](images/io_script.PNG)](images/io_script.PNG)

##### Public Tutorials and Additional Resources

For further learning and more detailed tutorials, consider the following public resources:

- **Grafana Tutorials:**  
  [Official Grafana Tutorials](https://grafana.com/tutorials) – Learn more about creating dashboards, panels, and advanced visualizations.

- **InfluxDB Documentation:**  
  [Official InfluxDB Docs](https://docs.influxdata.com/influxdb/) – Detailed guides on installation, configuration, and usage of InfluxDB.

- **Flux Language Guide:**  
  [InfluxDB Flux Documentation](https://docs.influxdata.com/flux/latest/) – In-depth information on the Flux query language and its capabilities.


#### Available Datapoints / Metrics

| Category      | Key Fields |
|----------------------|------------|
| **CPU**      | `usage_percent`, `user`, `system`, `idle`, `nice`, `iowait`, `irq`, `softirq`, `steal`, `guest`, `guest_nice`, `load1`, `load5`, `load15` |
| **Disk**     | `total`, `used`, `free`, `usage` `used percent`; tags: `device`, `fstype`, `mount` |
| **Metadata** | Based on event configuration |
| **Network**  | `bytes_sent`, `bytes_recv`, `packets_sent`, `packets_recv`, `err_in`, `err_out`, `drop_in`, `drop_out` |
| **Power**    | `current_power`, `average_power`, `max_power`, `pse_poe_class`, `lldp_poe_class`, `power_requested` |
| **Process**  | `cpu_percent`, `RSS`, `VMS`, `HWM`, `data`, `stack`, `locked`, `swap` |
| **Swap**      | `total`, `used`, `free`, `usage` `used percent` |
| **Uptime**   | `uptime` |
| **Virtual Memory**   | `total`, `available`, `used`, `used_percent`, `free`, `active`, `inactive`, `wired`, `buffers`, `cached`, `write_back`, `dirty`, `write_back_tmp`, `shared`, `slab`, `sreclaimable`, `sunreclaim`, `page_tables`, `swap_cached`, `commit_limit`, `committed_as`, `high_total`, `high_free`, `low_total`, `low_free`, `swap_total`, `swap_free`, `mapped`, `vmalloc_total`, `vmalloc_used`, `vmalloc_chunk`, `huge_pages_total`, `huge_pages_free`, `huge_pages_rsvd`, `huge_pages_surp`, `huge_page_size`, `anon_huge_pages` |