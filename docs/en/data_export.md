# Data Export

The **Data Export** section lets you export data from the Triel system into external databases. Exports can run manually or on a schedule: rows from a source table are transferred into a table of the target database according to a defined column mapping.

The section has two tabs:
* **Export Tasks** — configure and run data export jobs.
* **Connections** — configure connections to target databases.

Before creating an export task, you must create at least one database connection.

## Connections

The **Connections** tab lists the configured connections to external databases. Each connection shows its name, DB type, host, port, database name, and username.

Supported database types:
* PostgreSQL
* MySQL
* MS SQL Server
* Oracle

### Creating and editing a connection
When creating or editing a connection, fill in the following fields:
* `Name` — an arbitrary name to identify the connection.
* `DB Type` — the type of the target database (see the list above).
* `Host` — the database server address (host name or IP address).
* `Port` — the database server port (a number from 1 to 65535).
* `Database` — the database name.
* `Username` — the user name used to connect.
* `Password` — the user password. When editing, leave the field blank to keep the current password.
* `Extra JDBC properties` — an optional field for additional JDBC connection-string properties (see below).

> All fields except `Extra JDBC properties` are required. For security reasons, the password is never displayed after it has been saved.

#### Extra JDBC properties
The value of this field is appended by the server to the JDBC connection string unchanged. The separator between the database address and the properties is added automatically based on the DB type, so you **must not** include it at the start of the field:
* PostgreSQL, MySQL, and Oracle use the `?` separator, and multiple properties are joined with `&`;
* MS SQL Server uses the `;` separator, and multiple properties are also joined with `;`.

Examples of field values and the resulting connection strings:

* **PostgreSQL** — properties: `sslmode=require`

  ```
  jdbc:postgresql://10.0.0.5:5432/warehouse?sslmode=require
  ```

* **MySQL** — properties: `useSSL=false&allowPublicKeyRetrieval=true`

  ```
  jdbc:mysql://10.0.0.5:3306/warehouse?useSSL=false&allowPublicKeyRetrieval=true
  ```

* **MS SQL Server** — properties: `encrypt=false;trustServerCertificate=true`

  ```
  jdbc:sqlserver://10.0.0.5:1433;databaseName=warehouse;encrypt=false;trustServerCertificate=true
  ```

* **Oracle** — properties: `oracle.net.CONNECT_TIMEOUT=5000`

  ```
  jdbc:oracle:thin:@//10.0.0.5:1521/warehouse?oracle.net.CONNECT_TIMEOUT=5000
  ```

> The set of valid properties is defined by the driver of the corresponding database. The maximum field length is 1024 characters.

### Testing a connection
The connection context menu provides a **Test Connection** action. It performs a test connection to the database and reports whether the connection succeeded. It is recommended to test a connection after creating it and before using it in export tasks.

### Deleting a connection
A connection can be deleted from the context menu. If the connection is used by at least one export task, it cannot be deleted — delete or change the related tasks first.

## Export Tasks

The **Export Tasks** tab lists the configured export jobs. Each task shows its name, the connection it uses, the source table, the target table, the schedule, the enabled status, and the last-modified date.

### Creating and editing a task
When creating or editing a task, fill in the following fields:
* `Name` — an arbitrary name for the task.
* `Connection` — the connection to the target database (from the connections created earlier).
* `Source Table` — the Triel system table the data is exported from.
* `Target Table` — the target database table the data is written to. The list of tables is loaded automatically after a connection is selected.
* `Schedule` — how often the task runs automatically (see below).
* `Enabled` — a checkbox to enable/disable the task. A disabled task does not run on schedule.

#### Column mappings
A task requires a column mapping — which column of the source table is transferred into which column of the target table.
* To add a mapping, click **Add column** and select the source and target columns.
* At least one mapping must be defined.
* The same source or target column can be used only once.
* If the types of the source and target columns are incompatible, a warning is shown next to the mapping.

### Schedule
The schedule determines how often the task runs automatically:
* `Manually` — the task runs only on demand.
* `Every minute`
* `Every 5 minutes`
* `Every 10 minutes`
* `Every hour`
* `Every 12 hours`
* `Every 24 hours`

> The export is incremental: each run exports only the new rows added since the previous run. The system remembers the ID of the last exported row.

### Running a task manually
The task context menu provides a **Run Now** action. It immediately starts the export regardless of the schedule (confirmation is required).

### Run history
The **Run History** action opens a log of the task's past executions. Each run shows:
* `Status` — succeeded (`SUCCESS`) or failed (`FAILURE`).
* `Started At` — the execution start time.
* `Finished At` — the execution finish time.
* `Rows Exported` — the number of rows exported.
* `Error` — the error message if the run failed.

### Deleting a task
A task can be deleted from the context menu (confirmation is required).
