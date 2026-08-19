# Triel Pro Co Database Documentation

This document describes the database schema for the Triel Pro Co system.

Database engine: `PostgreSQL`

## Database Diagram

![Database Diagram](../images/db_diagram.jpg)

## Core Entities

### users
Stores user information and authentication details.
- `id`: Internal unique identifier (Long).
- `username`: Unique username for login.
- `password`: Hashed password.
- `first_name`, `last_name`: User's name.
- `role`: User role (e.g., ADMIN, WORKER).
- `is_active`: Flag indicating if the user account is active.

### category
Defines product categories and their default settings.
- `id`: Unique identifier (UUID String).
- `category_set_id`: Reference to the `category_set`.
- `name`: Category name.
- `image_path`: Path to the category icon/image.
- `external_id`: Identifier from external systems.
- `tare_weight`: Default tare weight for the product.
- `pack_tare_weight`: Default tare weight for the pack.
- `pallet_tare_weight`: Default tare weight for the pallet.
- `ignore_product_out_of_weight_range`: When `true`, the applicator rejects products whose net weight is outside [`product_min_weight`, `product_max_weight`].
- `product_min_weight`: Minimum allowed product net weight, grams.
- `product_max_weight`: Maximum allowed product net weight, grams.
- `is_deleted`: Soft delete flag.

### category_set
Groups categories together.
- `id`: Unique identifier (UUID String).
- `name`: Name of the set.
- `is_deleted`: Soft delete flag.

## Labeling & Printing

### label_template
Stores label designs and their associations.
- `id`: Unique identifier (UUID String).
- `category_set_id`: Optional reference to a `category_set`.
- `category_id`: Optional reference to a `category`.
- `template_variant_id`: Reference to the `label_template_variant`.
- `dimension_id`: Reference to the `label_dimension`.
- `name`: Template name.
- `content`: Design content (usually JSON or XML).
- `fallback`: Flag indicating if this is a fallback template.
- `product_packaging_type`: Type of packaging (SINGLE_PRODUCT, PACK, PALLET).
- `rotation`: Design rotation (0, 90, 180, 270).
- `created_at`, `updated_at`: Audit timestamps.

### label_template_variant
Defines specific variants of a template.
- `id`: Unique identifier (String).
- `name`: Variant name.

### label_dimension
Physical dimensions of labels.
- `id`: Unique identifier (String).
- `width`: Label width in mm.
- `height`: Label height in mm.

### template_variable
Dynamic variables used within label designs.
- `id`: Unique identifier (UUID String).
- `category_set_id`: Optional reference to a `category_set`.
- `category_id`: Optional reference to a `category`.
- `key`: Variable name (e.g., `EXPIRY_DATE`).
- `value`: Static value or formula.
- `is_hot`: Flag for variables that are frequently updated.

### counter
Defines sequential counters.
- `id`: Unique identifier (UUID String).
- `key`: Counter key.
- `category_set_id`, `category_id`: Optional scoping.
- `product_packaging_type`: Packaging type associated with the counter.
- `min_value`, `max_value`: Counter range.

### counter_value
Current state of a counter for a specific machine.
- `id`: Unique identifier (UUID String).
- `counter_id`: Reference to the `counter`.
- `machine_id`: Identifier of the machine using this counter.
- `value`: Current counter value.

### unique_code
Pool of unique codes (e.g., DataMatrix) uploaded to the system for future printing.
- `id`: Unique identifier (UUID String).
- `category_id`: Reference to the `category`.
- `code`: The code value itself.
- `created_at`: Timestamp when the code was uploaded.
- `created_by`: User who uploaded the code.
- `reserved`: Flag indicating if the code is reserved for printing.
- `reserved_by`: Identifier of the process or machine that reserved the code.
- `reserved_at`: Reservation timestamp.
- `parts`: JSONB field containing parsed parts of the code (GTIN, serial number, etc.).

### printed_unique_code
Registry of codes that have actually been printed on products.
- `id`: Unique identifier.
- `code`: The code value (indexed for fast lookup).
- `category_id`: Reference to the `category`.
- `external_product_id`: Reference to the produced item (ID from `final_product`).
- `machine_id`: ID of the machine that performed printing.
- `validated`: Flag indicating if the code was successfully verified by a scanner after printing.
- `source_code_deleted`: Flag indicating if the original code was removed from the `unique_code` table after use.
- `created_at`, `created_by`: Audit timestamps and author.
- `reserved`, `reserved_by`, `reserved_at`: Reservation details during printing.
- `printed_at`: Timestamp of actual printing.

### print_task_log
Execution logs for print operations.
- `id`: Unique identifier (Long).
- `machine_id`: ID of the machine that performed printing.
- `status`: Operation status (SUCCESS, ERROR).
- `data`: JSONB field with task details and results.
- `created_at`: Log timestamp.

## Production Data

All production data tables (`final_product`, `product_pack`, `product_pallet`) include common fields from a base product definition:
- `machine_id`: ID of the machine that produced the item.
- `category_id`, `template_id`: Identifiers of the category and template used at the time of production.
- `created_at`, `updated_at`: Production and modification timestamps.
- `rendered_barcodes`: JSONB field containing generated barcode data.

### product_batch
Represents a production run/batch.
- `id`: Internal unique identifier (Integer).
- `name`: Batch name. Globally unique across all batches, closed ones included. Nullable in the schema for legacy rows, but required by the API.
- `external_id`: Identifier from external systems. Globally unique across all batches, closed ones included. Nullable in the schema for legacy rows, but required by the API.
- `start_time`, `end_time`: Batch duration.
- `active`: Flag indicating an active batch. **Several batches may be active at the same time** — one per production line. Each applicator panel selects which active batch it produces into.
- `products_count`: Total number of products in the batch.
- `products_net_weight`, `products_gross_weight`, `products_tare_weight`: Aggregated weights, recalculated from `final_product` rows carrying this batch id.
- `reports_generated`, `products_report_path`, `batch_report_path`: CSV reports produced when the batch is closed.

### product_pallet
Represents a shipping or storage pallet.
- `id`: Internal unique identifier (Long).
- `external_id`: Identifier from external systems.
- `active`: Flag indicating if the pallet is currently being filled.
- `max_weight`, `max_product_count`: Limits for the pallet.
- `product_count`, `product_pack_count`: Current counts.
- `products_net_weight`, `products_gross_weight`, `products_tare_weight`: Aggregated product weights.
- `product_packs_tare_weight`: Total weight of packs on the pallet.
- `tare_weight`: Weight of the pallet itself.

### product_pack
Represents a package of products.
- `id`: Internal unique identifier (Long).
- `external_id`: Identifier from external systems.
- `external_pallet_id`: Reference to the parent pallet.
- `active`: Flag indicating if the pack is currently being filled.
- `product_count`, `max_product_count`: Current and max number of items.
- `products_net_weight`, `products_gross_weight`, `products_tare_weight`: Aggregated item weights.
- `tare_weight`: Weight of the package itself.
- `printed`: Flag indicating if the pack label was printed.

### final_product
Individual labeled products.
- `id`: Internal unique identifier (Long).
- `external_id`: Identifier from external systems.
- `external_product_pack_id`: Reference to the parent pack.
- `external_product_pallet_id`: Reference to the parent pallet.
- `product_batch_id`: Reference to the `product_batch`.
- `initial_weight`: Weight before processing.
- `net_weight`, `gross_weight`, `tare_weight`: Recorded weights.
- `printed`: Flag indicating if the label was printed.

## System

### equipment
Registry of hardware instances and their status.
- `id`: Unique identifier (Long).
- `eureka_instance_id`: ID from Eureka discovery. Not unique — it moves between rows when a machine re-registers under a new instance ID.
- `status`: Instance status (UP, DOWN, etc.).
- `instance_type`: Type of equipment (e.g., LABEL_APPLICATOR, CONVEYOR).
- `internal_id`: Internal equipment identifier.
- `machine_id`: Logical machine ID. Unique and not null — the key equipment is upserted by; falls back to `eureka_instance_id` when the instance reports no machine ID.
- `host_name`, `ip_addr`, `port`: Network information.

## Data Export

### export_connection
Target database connections for data export.
- `id`: Unique identifier (UUID String).
- `name`: Unique connection name.
- `db_type`: Target database type (POSTGRESQL, MYSQL, MSSQL, ORACLE).
- `host`, `port`, `database_name`: Target database location.
- `username`, `password`: Target database credentials.
- `properties`: Optional extra JDBC URL parameters.
- `created_at`, `updated_at`: Audit timestamps.

### export_task
Scheduled data export task definitions.
- `id`: Unique identifier (UUID String).
- `name`: Unique task name.
- `connection_id`: Reference to the `export_connection`.
- `source_table`: Name of the source table/view, one of those configured in `export.tables`.
- `target_table`: Table name in the target database, optionally schema-qualified. `NULL` for file targets.
- `delimiter`: Field delimiter of the exported file (COMMA, PIPE, TAB). Used by file targets only.
- `file_mask`: Optional file name prefix; the file name is the mask plus a `yyMMddHHmmss` timestamp.
- `file_extension`: Extension of the exported file (TXT, CSV, PSV, TSV). Defaults to the delimiter's extension.
- `start_header`: Optional marker written as the first cell of the header row. Applies to TXT exports only; it has no data column, so data rows are not padded for it.
- `end_header`: Optional marker written as the last cell of the header row. Applies to TXT exports only, same as `start_header`.
- `header_in_each_row`: When `true`, the header row is repeated before every data row. Applies to TXT exports only.
- `column_mappings`: JSONB array of source-to-target column mappings.
- `cron_expression`: Spring 6-field cron schedule. `NULL` means the task is manual-only and never runs on a schedule.
- `enabled`: Flag indicating if the task is scheduled.
- `last_exported_id`: Export watermark — the highest source row `id` already exported.
- `created_at`, `updated_at`: Audit timestamps.

### export_task_log
Run history of export tasks.
- `id`: Unique identifier (Long).
- `task_id`: Reference to the `export_task` (rows are deleted with the task).
- `status`: Run status (SUCCESS, FAILED).
- `started_at`, `finished_at`: Run duration.
- `rows_exported`: Number of rows inserted into the target table.
- `from_id`, `to_id`: Watermark range covered by the run.
- `error_message`: Failure details, if any.

## Key Relationships

- **Categorization**: `category` and `label_template` are organized into `category_set` groups.
- **Labeling Hierarchy**: `label_template` links to `label_dimension` for physical sizing and `label_template_variant` for specific design variations.
- **Production Hierarchy**:
  - `product_batch` (Top level; several batches may be active concurrently, one per line)
  - `product_pallet` (Contains packs or products)
  - `product_pack` (Contains products, belongs to a pallet)
  - `final_product` (Leaf level, links to batch, pack, and pallet)
- **Unique Codes**:
  - `unique_code` stores a pool of available codes for categories.
  - `printed_unique_code` links a used code to a specific item (`final_product`).
- **Counters**: `counter_value` tracks the state of a `counter` per machine.
