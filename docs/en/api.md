# Triel Pro Co API Documentation

- [Authentication](#authentication)
  - [Obtain Access Token](#obtain-access-token)
  - [Using the Token](#using-the-token)
- [User Management](#user-management)
  - [List Users](#list-users)
  - [List Workers](#list-workers)
  - [Get User](#get-user)
  - [Create User](#create-user)
  - [Update User](#update-user)
  - [Delete User](#delete-user)
- [Category Management](#category-management)
  - [Category Sets](#category-sets)
    - [List Category Sets](#list-category-sets)
    - [Get Category Set](#get-category-set)
    - [Create Category Set](#create-category-set)
    - [Update Category Set](#update-category-set)
    - [Delete Category Set](#delete-category-set)
  - [Categories](#categories)
    - [List Categories in Set](#list-categories-in-set)
    - [Get Category](#get-category)
    - [Create Category](#create-category)
    - [Update Category](#update-category)
    - [Delete Category](#delete-category)
  - [Unique Codes](#unique-codes)
    - [Upload Unique Codes (JSON)](#upload-unique-codes-json)
    - [Upload Unique Codes (File)](#upload-unique-codes-file)
    - [List Unique Codes](#list-unique-codes)
    - [Reserve Unique Codes](#reserve-unique-codes)
    - [Download Unique Codes (CSV)](#download-unique-codes-csv)
    - [Unique Code Statistics](#unique-code-statistics)
    - [Delete Unique Codes](#delete-unique-codes)
    - [Upsert Printed Unique Codes](#upsert-printed-unique-codes)
    - [List Printed Unique Codes](#list-printed-unique-codes)
    - [Download Printed Unique Codes (CSV)](#download-printed-unique-codes-csv)
- [Label & Template Management](#label--template-management)
  - [Label Templates](#label-templates)
    - [List Templates](#list-templates)
    - [Find Template](#find-template)
    - [Get Template](#get-template)
    - [Create Template](#create-template)
    - [Update Template](#update-template)
    - [Delete Template](#delete-template)
    - [Set Fallback](#set-fallback)
    - [Render Template](#render-template)
  - [Template Variants](#template-variants)
    - [List Variants](#list-variants)
    - [Get Variant](#get-variant)
    - [Create Variant](#create-variant)
    - [Update Variant](#update-variant)
    - [Delete Variant](#delete-variant)
  - [Label Dimensions](#label-dimensions)
    - [List Dimensions](#list-dimensions)
    - [Get Dimension](#get-dimension)
    - [Create Dimension](#create-dimension)
    - [Update Dimension](#update-dimension)
    - [Delete Dimension](#delete-dimension)
- [Variable Management](#variable-management)
  - [List Variables](#list-variables)
  - [Create Variable](#create-variable)
  - [Update Variable](#update-variable)
  - [Patch Variables](#patch-variables)
  - [Delete Variable](#delete-variable)
  - [List Placeholders](#list-placeholders)
- [Product Batch Management](#product-batch-management)
  - [List Batches](#list-batches)
  - [Get Active Batch](#get-active-batch)
  - [Open Batch](#open-batch)
  - [Close Batch](#close-batch)
  - [Batch Report](#batch-report)
- [Counters](#counters)
  - [List Counters](#list-counters)
  - [Create Counter](#create-counter)
  - [Update Counter](#update-counter)
  - [Delete Counter](#delete-counter)
  - [List Counter Keys](#list-counter-keys)
  - [Search Counters](#search-counters)
  - [Get Counter Values](#get-counter-values)
  - [Reset Counter Value](#reset-counter-value)
- [Equipment](#equipment)
  - [List Equipment](#list-equipment)
- [Printing & Rendering](#printing--rendering)
  - [Submit Print Task](#submit-print-task)
  - [Print Task Logs](#print-task-logs)
  - [Render Barcode](#render-barcode)

## Authentication

The API uses JWT (JSON Web Token) for authentication. All endpoints (except `/login`) require a valid token to be passed in the `Authorization` header.

### Obtain Access Token

To get an access token, use the login endpoint.

**Endpoint**: `POST /api/v1/auth/login`  
**Description**: Authenticates a user and returns a JWT.

**Request Body**:
```json
{
  "username": "admin",
  "password": "password"
}
```

**Response**:
```json
{
  "jwt": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImlhdCI6MTYy..."
}
```

### Using the Token

Once you have the JWT, include it in the `Authorization` header of every request:

```text
Authorization: Bearer <your_access_token>
```

---

## User Management

Endpoints for managing system users and workers.

### List Users
`GET /api/v1/users`  
Returns a list of all users.

**Response**:
```json
[
  {
    "id": 1,
    "username": "admin",
    "firstName": "Admin",
    "lastName": "System",
    "role": "ADMIN",
    "userType": "HUMAN"
  },
  {
    "id": 2,
    "username": "worker1",
    "firstName": "John",
    "lastName": "Doe",
    "role": "WORKER",
    "userType": "HUMAN"
  }
]
```

### List Workers
`GET /api/v1/users/workers`  
Returns a list of users with the `WORKER` role and `HUMAN` type.

**Response**:
```json
[
  {
    "id": 2,
    "username": "worker1",
    "firstName": "John",
    "lastName": "Doe",
    "role": "WORKER",
    "userType": "HUMAN"
  }
]
```

### Get User
`GET /api/v1/users/{id}`  
Returns details of a specific user by ID.

**Response**:
```json
{
  "id": 1,
  "username": "admin",
  "firstName": "Admin",
  "lastName": "System",
  "role": "ADMIN",
  "userType": "HUMAN"
}
```

### Create User
`POST /api/v1/users`  
Creates a new user.

**Request Body**:
```json
{
  "username": "jdoe",
  "firstName": "John",
  "lastName": "Doe",
  "password": "securepassword",
  "role": "WORKER",
  "userType": "HUMAN"
}
```

**Response**:
```json
{
  "id": 3,
  "username": "jdoe",
  "firstName": "John",
  "lastName": "Doe",
  "role": "WORKER",
  "userType": "HUMAN"
}
```

### Update User
`PUT /api/v1/users/{id}`  
Updates an existing user.

**Request Body**:
```json
{
  "username": "jdoe_updated",
  "firstName": "John",
  "lastName": "Smith",
  "password": "newpassword",
  "role": "WORKER",
  "userType": "HUMAN"
}
```

**Response**:
```json
{
  "id": 3,
  "username": "jdoe_updated",
  "firstName": "John",
  "lastName": "Smith",
  "role": "WORKER",
  "userType": "HUMAN"
}
```

### Delete User
`DELETE /api/v1/users/{id}`  
Removes a user from the system. Users cannot delete themselves.

---

## Category Management

Manage category sets and individual categories. Categories are used to define product types, their tare weights, and associated images.

### Category Sets

#### List Category Sets
`GET /api/v1/category-sets`  
Returns a list of all category sets.

**Response**:
```json
[
  {
    "id": "set_1",
    "name": "Dairy Products",
    "deleted": false
  }
]
```

#### Get Category Set
`GET /api/v1/category-sets/{id}`  
Returns a specific category set.

**Response**:
```json
{
  "id": "set_1",
  "name": "Dairy Products",
  "deleted": false
}
```

#### Create Category Set
`POST /api/v1/category-sets`  
Creates a new category set.

**Request Body**:
```json
{
  "id": "set_1",
  "name": "Dairy Products"
}
```

**Response**:
```json
{
  "id": "set_1",
  "name": "Dairy Products",
  "deleted": false
}
```

#### Update Category Set
`PUT /api/v1/category-sets/{id}`  
Updates an existing category set.

**Request Body**:
```json
{
  "name": "Updated Dairy Products"
}
```

**Response**:
```json
{
  "id": "set_1",
  "name": "Updated Dairy Products",
  "deleted": false
}
```

#### Delete Category Set
`DELETE /api/v1/category-sets/{id}`  
Deletes a category set.

**Response**:
```json
{
  "id": "set_1",
  "name": "Updated Dairy Products",
  "deleted": true
}
```

### Categories

#### List Categories in Set
`GET /api/v1/category-sets/{categorySetId}/categories`  
Returns all categories belonging to a specific set.

**Response**:
```json
[
  {
    "id": "cat_1",
    "category_set": {
      "id": "set_1",
      "name": "Dairy Products",
      "deleted": false
    },
    "name": "Milk 3.2%",
    "external_id": "EXT-001",
    "tare_weight": 50,
    "pack_tare_weight": 200,
    "pallet_tare_weight": 5000,
    "has_image": true,
    "image_url": "/api/v1/categories/cat_1/image"
  }
]
```

#### Get Category
`GET /api/v1/categories/{id}`  
Returns details for a specific category.

**Response**:
```json
{
  "id": "cat_1",
  "category_set": {
    "id": "set_1",
    "name": "Dairy Products",
    "deleted": false
  },
  "name": "Milk 3.2%",
  "external_id": "EXT-001",
  "tare_weight": 50,
  "pack_tare_weight": 200,
  "pallet_tare_weight": 5000,
  "has_image": true,
  "image_url": "/api/v1/categories/cat_1/image"
}
```

#### Create Category
`POST /api/v1/category-sets/{categorySetId}/categories`  
Creates a new category within a set. Content type: `multipart/form-data`.

**Parameters**:
- `id` (string): Unique identifier.
- `name` (string): Display name.
- `externalId` (string): External system reference.
- `tareWeight` (int): Weight of the single product container.
- `packTareWeight` (int): Weight of the pack container.
- `palletTareWeight` (int): Weight of the pallet container.
- `image` (file): Optional image file.

**Response**:
```json
{
  "id": "cat_1",
  "category_set": {
    "id": "set_1",
    "name": "Dairy Products",
    "deleted": false
  },
  "name": "Milk 3.2%",
  "external_id": "EXT-001",
  "tare_weight": 50,
  "pack_tare_weight": 200,
  "pallet_tare_weight": 5000,
  "has_image": true,
  "image_url": "/api/v1/categories/cat_1/image"
}
```

#### Update Category
`PUT /api/v1/categories/{id}`  
Updates an existing category. Content type: `multipart/form-data`.

**Response**:
```json
{
  "id": "cat_1",
  "category_set": {
    "id": "set_1",
    "name": "Dairy Products",
    "deleted": false
  },
  "name": "Milk 3.2% Updated",
  "external_id": "EXT-001",
  "tare_weight": 55,
  "pack_tare_weight": 210,
  "pallet_tare_weight": 5100,
  "has_image": true,
  "image_url": "/api/v1/categories/cat_1/image"
}
```

#### Delete Category
`DELETE /api/v1/categories/{id}`  
Deletes a category.

### Unique Codes

#### Upload Unique Codes (JSON)
`POST /api/v1/categories/{categoryId}/unique-codes/upload/json`  
Uploads a list of unique codes in JSON format.

**Request Body**:
```json
["CODE1", "CODE2", "CODE3"]
```

**Response**:
```json
{
  "uploaded_count": 3,
  "ignored_count": 0
}
```

#### Upload Unique Codes (File)
`POST /api/v1/categories/{categoryId}/unique-codes/upload/file`  
Uploads unique codes from a file. Content type: `multipart/form-data`.

**Parameters**:
- `file` (file): File containing unique codes (one per line).

**Response**:
```json
{
  "uploaded_count": 100,
  "ignored_count": 2
}
```

#### List Unique Codes
`GET /api/v1/categories/{categoryId}/unique-codes`  
Returns a paginated list of all available unique codes for a specific category.

**Query Parameters**:
- `page`, `size`: Pagination parameters.

**Response**:
```json
{
  "content": [
    {
      "id": "code_1",
      "categoryId": "cat_1",
      "code": "010460...21...",
      "parts": {
        "01": "0460...",
        "21": "..."
      }
    }
  ],
  "pageable": {
    "sort": {
      "sorted": false,
      "unsorted": true,
      "empty": true
    },
    "offset": 0,
    "pageNumber": 0,
    "pageSize": 20,
    "paged": true,
    "unpaged": false
  },
  "totalPages": 1,
  "totalElements": 1,
  "last": true,
  "size": 20,
  "number": 0,
  "sort": {
    "sorted": false,
    "unsorted": true,
    "empty": true
  },
  "numberOfElements": 1,
  "first": true,
  "empty": false
}
```

#### Download Unique Codes (CSV)
`GET /api/v1/categories/{categoryId}/unique-codes/download/csv`  
Downloads available unique codes as a CSV file.

**Response**:
CSV file download.


#### Unique Code Statistics
`GET /api/v1/categories/{categoryId}/unique-codes/stats`  
Returns statistics for unique codes of a specific category.

**Response**:
```json
{
  "totalCount": 1000,
  "reservedCount": 50,
  "printedCount": 25000
}
```

#### Delete Unique Codes
`DELETE /api/v1/categories/{categoryId}/unique-codes`  
Deletes all available unique codes for a specific category.

**Response**:
```json
{
  "deletedCount": 100
}
```

#### Upsert Printed Unique Codes
`POST /api/v1/categories/{categoryId}/printed/upsert`  
Updates or inserts printed unique codes.

**Request Body**:
A list of `PrintedUniqueCodeDto` objects.
```json
[
  {
    "id": "code-id-1",
    "code": "010460...21...",
    "categoryId": "cat_1",
    "machineId": "MACHINE-01",
    "printedAt": "2023-10-27T10:00:00",
    "validated": true
  }
]
```

**Response**:
200 OK

#### List Printed Unique Codes
`GET /api/v1/categories/{categoryId}/unique-codes/printed`  
Returns a paginated list of printed unique codes.

**Query Parameters**:
- `startDate` (ISO Date Time): Optional start date filter.
- `endDate` (ISO Date Time): Optional end date filter.
- `page`, `size`, `sort`: Pagination parameters.

**Response**:
Standard Spring Data Page object containing `PrintedUniqueCodeDto` items.

#### Download Printed Unique Codes (CSV)
`GET /api/v1/categories/{categoryId}/unique-codes/printed/download/csv`  
Downloads printed unique codes as a CSV file.

**Query Parameters**:
- `startDate` (ISO Date Time): Optional start date filter.
- `endDate` (ISO Date Time): Optional end date filter.

**Response**:
CSV file download.

---

## Label & Template Management

Endpoints for managing label templates and their configurations.

### Label Templates

#### List Templates
`GET /api/v1/label-templates/list`  
Returns a list of templates. Supports `searchCriteria` query parameter.

**Response**:
```json
[
  {
    "id": "tmpl_1",
    "name": "Single Product Label",
    "content": "<xml>...</xml>",
    "category_id": "cat_1",
    "product_packaging_type": "SINGLE_PRODUCT",
    "dimension": {
      "id": 1,
      "width": 100,
      "height": 50
    },
    "is_fallback": false
  }
]
```

#### Find Template
`GET /api/v1/label-templates/find`  
Finds a template based on category, set, and packaging type.
Query Parameters: `categoryId`, `categorySetId`, `packagingType`.

**Response**:
```json
{
  "id": "tmpl_1",
  "name": "Single Product Label",
  "content": "<xml>...</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "id": 1,
    "width": 100,
    "height": 50
  },
  "is_fallback": false
}
```

#### Get Template
`GET /api/v1/label-templates/{id}`  
Returns a specific template.

**Response**:
```json
{
  "id": "tmpl_1",
  "name": "Single Product Label",
  "content": "<xml>...</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "id": 1,
    "width": 100,
    "height": 50
  },
  "is_fallback": false
}
```

#### Create Template
`POST /api/v1/label-templates`  
Creates a new label template.

**Request Body**:
```json
{
  "name": "Single Product Label",
  "content": "<xml>...</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "width": 100,
    "height": 50
  }
}
```

**Response**:
```json
{
  "id": "tmpl_2",
  "name": "Single Product Label",
  "content": "<xml>...</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "id": 2,
    "width": 100,
    "height": 50
  },
  "is_fallback": false
}
```

#### Update Template
`PUT /api/v1/label-templates/{id}`  
Updates an existing template.

**Request Body**:
```json
{
  "name": "Updated Label",
  "content": "<xml>new content</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "width": 100,
    "height": 60
  }
}
```

**Response**:
```json
{
  "id": "tmpl_2",
  "name": "Updated Label",
  "content": "<xml>new content</xml>",
  "category_id": "cat_1",
  "product_packaging_type": "SINGLE_PRODUCT",
  "dimension": {
    "id": 3,
    "width": 100,
    "height": 60
  },
  "is_fallback": false
}
```

#### Delete Template
`DELETE /api/v1/label-templates/{id}`  
Deletes a template.

#### Set Fallback
`POST /api/v1/label-templates/{id}/fallback`  
Sets the specified template as the fallback template.

#### Render Template
`GET /api/v1/label-templates/{id}/render`  
Renders a template as a JPEG image.

**Response**:
Binary image data (JPEG).

### Template Variants

#### List Variants
`GET /api/v1/label-template-variants`  
Returns all label template variants.

**Response**:
```json
[
  {
    "id": "var_1",
    "name": "Variant A"
  }
]
```

#### Get Variant
`GET /api/v1/label-template-variants/{id}`  
Returns a specific variant.

**Response**:
```json
{
  "id": "var_1",
  "name": "Variant A"
}
```

#### Create Variant
`POST /api/v1/label-template-variants`  
Creates a new variant.

**Request Body**:
```json
{
  "id": "var_2",
  "name": "Variant B"
}
```

**Response**:
```json
{
  "id": "var_2",
  "name": "Variant B"
}
```

#### Update Variant
`PUT /api/v1/label-template-variants/{id}`  
Updates a variant.

**Request Body**:
```json
{
  "name": "Variant B Updated"
}
```

**Response**:
```json
{
  "id": "var_2",
  "name": "Variant B Updated"
}
```

#### Delete Variant
`DELETE /api/v1/label-template-variants/{id}`  
Deletes a variant.

### Label Dimensions

#### List Dimensions
`GET /api/v1/label-templates/dimension`  
Returns all defined label dimensions.

**Response**:
```json
[
  {
    "id": 1,
    "width": 100,
    "height": 50
  }
]
```

#### Get Dimension
`GET /api/v1/label-templates/dimension/{id}`  
Returns a specific dimension.

**Response**:
```json
{
  "id": 1,
  "width": 100,
  "height": 50
}
```

#### Create Dimension
`POST /api/v1/label-templates/dimension`  
Creates a new dimension.

**Request Body**:
```json
{
  "width": 80,
  "height": 40
}
```

**Response**:
```json
{
  "id": 2,
  "width": 80,
  "height": 40
}
```

#### Update Dimension
`PUT /api/v1/label-templates/dimension/{id}`  
Updates an existing dimension.

**Request Body**:
```json
{
  "width": 85,
  "height": 45
}
```

**Response**:
```json
{
  "id": 2,
  "width": 85,
  "height": 45
}
```

#### Delete Dimension
`DELETE /api/v1/label-templates/dimension/{id}`  
Deletes a dimension.

---

## Variable Management

Manage dynamic variables and placeholders used in label templates.

### List Variables
`GET /api/v1/variables/list`  
Returns a list of template variables. Can be filtered by `searchCriteria`, `hot` (boolean), or `categoryId`.

**Response**:
```json
[
  {
    "id": "var_123",
    "category_set_id": "set_1",
    "category_set_name": "Dairy Products",
    "category_id": "cat_1",
    "category_name": "Milk 3.2%",
    "key": "expiration_days",
    "value": "7",
    "hot": true
  }
]
```

### Create Variable
`POST /api/v1/variables`  
Creates a new template variable.

**Request Body**:
```json
{
  "category_set_id": "set_1",
  "category_id": "cat_1",
  "key": "expiration_days",
  "value": "7",
  "hot": true
}
```

**Response**:
```json
{
  "id": "var_124",
  "category_set_id": "set_1",
  "category_set_name": "Dairy Products",
  "category_id": "cat_1",
  "category_name": "Milk 3.2%",
  "key": "expiration_days",
  "value": "7",
  "hot": true
}
```

### Update Variable
`PUT /api/v1/variables/{id}`  
Updates a variable.

**Request Body**:
```json
{
  "value": "10",
  "hot": false
}
```

**Response**:
```json
{
  "id": "var_124",
  "category_set_id": "set_1",
  "category_set_name": "Dairy Products",
  "category_id": "cat_1",
  "category_name": "Milk 3.2%",
  "key": "expiration_days",
  "value": "10",
  "hot": false
}
```

### Patch Variables
`PATCH /api/v1/variables`  
Bulk creates or updates multiple variables.

**Request Body**:
```json
[
  {
    "category_id": "cat_1",
    "key": "line_id",
    "value": "Line 1"
  },
  {
    "id": "var_124",
    "value": "14"
  }
]
```

**Response**:
```json
[
  {
    "id": "var_125",
    "category_id": "cat_1",
    "key": "line_id",
    "value": "Line 1",
    "hot": false
  },
  {
    "id": "var_124",
    "category_id": "cat_1",
    "key": "expiration_days",
    "value": "14",
    "hot": false
  }
]
```

### Delete Variable
`DELETE /api/v1/variables/{id}`  
Deletes a variable.

### List Placeholders
`GET /api/v1/variables/placeholders`  
Returns all available placeholders (keys) that can be used in templates.

**Response**:
```json
[
  {
    "variable": "expiration_days",
    "variable_type": "STRING",
    "type": "VARIABLE"
  },
  {
    "variable": "current_date",
    "variable_type": "DATE",
    "type": "SYSTEM"
  }
]
```

---

## Product Batch Management

Manage production batches. A batch tracks the production of a specific product over time, recording counts and weights.

### List Batches
`GET /api/v1/product-batches`  
Returns a list of all product batches.

**Response**:
```json
[
  {
    "id": 1,
    "name": "Batch 2023-01",
    "external_id": "EXT-B-01",
    "start": "2023-01-01T08:00:00",
    "end": "2023-01-01T17:00:00",
    "active": false,
    "products_count": 500,
    "products_net_weight": 250000,
    "products_gross_weight": 275000,
    "products_tare_weight": 25000,
    "created_at": "2023-01-01T08:00:00",
    "updated_at": "2023-01-01T17:00:00"
  }
]
```

### Get Active Batch
`GET /api/v1/product-batches/active`  
Returns details of the currently active batch.

**Response**:
```json
{
  "id": 2,
  "name": "Batch 2023-02",
  "external_id": "EXT-B-02",
  "start": "2023-01-02T08:00:00",
  "active": true,
  "products_count": 50,
  "created_at": "2023-01-02T08:00:00"
}
```

### Open Batch
`POST /api/v1/product-batches/open`  
Opens a new product batch.
Query Parameters: `name` (optional), `externalId` (optional).

**Response**:
```json
{
  "id": 2,
  "name": "Batch 2023-02",
  "external_id": "EXT-B-02",
  "start": "2023-01-02T08:00:00",
  "active": true,
  "products_count": 0,
  "created_at": "2023-01-02T08:00:00"
}
```

### Close Batch
`POST /api/v1/product-batches/close`  
Closes the currently active batch.

### Batch Report
`GET /api/v1/product-batches/{id}/report`  
Generates a production report for the batch.
Query Parameters: `type` (e.g., `PDF`, `EXCEL`).

**Response**:
Binary report file.

---

## Counters

Manage production counters for tracking quantities across different machines and categories.

### List Counters
`GET /api/v1/counters/list`  
Returns all registered counters.

**Response**:
```json
[
  {
    "id": "cnt_1",
    "name": "Main Production Counter",
    "key": "prod_total",
    "reset_value": 0,
    "category_id": "cat_1",
    "category_set_id": "set_1",
    "product_packaging_type": "SINGLE_PRODUCT"
  }
]
```

### Create Counter
`POST /api/v1/counters`  
Creates a new counter.

**Request Body**:
```json
{
  "name": "Line 2 Counter",
  "key": "line2_total",
  "reset_value": 0,
  "category_id": "cat_1",
  "category_set_id": "set_1",
  "product_packaging_type": "SINGLE_PRODUCT"
}
```

**Response**:
```json
{
  "id": "cnt_2",
  "name": "Line 2 Counter",
  "key": "line2_total",
  "reset_value": 0,
  "category_id": "cat_1",
  "category_set_id": "set_1",
  "product_packaging_type": "SINGLE_PRODUCT"
}
```

### Update Counter
`PUT /api/v1/counters/{id}`  
Updates an existing counter.

**Request Body**:
```json
{
  "name": "Line 2 Counter Updated",
  "reset_value": 10
}
```

**Response**:
```json
{
  "id": "cnt_2",
  "name": "Line 2 Counter Updated",
  "key": "line2_total",
  "reset_value": 10,
  "category_id": "cat_1",
  "category_set_id": "set_1",
  "product_packaging_type": "SINGLE_PRODUCT"
}
```

### Delete Counter
`DELETE /api/v1/counters/{id}`  
Deletes a counter.

### List Counter Keys
`GET /api/v1/counters/keys`  
Returns a list of all unique counter keys.

**Response**:
```json
["prod_total", "line2_total"]
```

### Search Counters
`GET /api/v1/counters/search`  
Finds counters matching specific criteria.
Query Parameters: `categoryId`, `categorySetId`, `machineId`, `packagingType` (optional).

**Response**:
```json
[
  {
    "id": "cnt_1",
    "name": "Main Production Counter",
    "key": "prod_total",
    "value": 150
  }
]
```

### Get Counter Values
`GET /api/v1/counters/{counterId}/values`  
Returns values for a specific counter across all machines.

**Response**:
```json
[
  {
    "counter_id": "cnt_1",
    "machine_id": "machine_A",
    "value": 150
  },
  {
    "counter_id": "cnt_1",
    "machine_id": "machine_B",
    "value": 45
  }
]
```

### Reset Counter Value
`POST /api/v1/counters/{counterId}/values/{machineId}/reset`  
Resets the value of a specific counter on a specific machine.

---

## Equipment

Manage hardware and monitor equipment status.

#### List Equipment
`GET /api/v1/equipment`  
Returns a list of all registered equipment and their current statuses.

**Response**:
```json
[
  {
    "id": 1,
    "eurekaInstanceId": "applicator:8081",
    "status": "UP",
    "instanceType": "APPLICATOR",
    "internalId": "APP-01",
    "machineId": "LINE-1",
    "hostName": "applicator-1.local",
    "ipAddr": "192.168.1.50",
    "port": 8081,
    "createdAt": "2023-01-01T10:00:00"
  }
]
```

---

## Printing & Rendering

Endpoints for submitting print jobs and rendering barcodes.

### Submit Print Task
`POST /api/v1/tasks/print-task`  
Submits a print job to one or more equipment instances.

**Request Body**:
```json
{
  "equipmentIds": [1, 2],
  "printTaskRequest": {
    "productCategoryId": "cat_123",
    "productTemplateId": "tmpl_456",
    "productPack": {
      "enabled": true,
      "itemsCountUnlimited": false,
      "maxProductCount": 12,
      "maxWeightUnlimited": true,
      "maxWeight": 0,
      "templateId": "tmpl_pack_789"
    },
    "productPallet": {
      "enabled": false,
      "itemsCountUnlimited": true,
      "maxProductCount": 0,
      "maxWeightUnlimited": true,
      "maxWeight": 0,
      "templateId": "tmpl_pallet_012"
    }
  }
}
```

**Response**:
```json
{
  "results": {
    "1": {
      "success": true,
      "statusCode": 200
    },
    "2": {
      "success": false,
      "errorMessage": "Equipment status is DOWN"
    }
  }
}
```

### Print Task Logs
`GET /api/v1/tasks/log`  
Returns a history of submitted print tasks.

**Response**:
```json
[
  {
    "id": 101,
    "overallStatus": "PARTIAL_SUCCESS",
    "request": {
      "equipmentIds": [1, 2],
      "printTaskRequest": {
        "productCategoryId": "cat_123",
        "productTemplateId": "tmpl_456",
        "productPack": {
          "enabled": true,
          "itemsCountUnlimited": false,
          "maxProductCount": 12,
          "maxWeightUnlimited": true,
          "maxWeight": 0,
          "templateId": "tmpl_pack_789"
        },
        "productPallet": {
          "enabled": false,
          "itemsCountUnlimited": true,
          "maxProductCount": 0,
          "maxWeightUnlimited": true,
          "maxWeight": 0,
          "templateId": "tmpl_pallet_012"
        }
      }
    },
    "response": {
      "results": {
        "1": { "success": true, "statusCode": 200 },
        "2": { "success": false, "errorMessage": "..." }
      }
    },
    "createdAt": "2023-01-01T12:00:00",
    "createdBy": {
      "id": 1,
      "username": "admin",
      "firstName": "Admin",
      "lastName": "System",
      "role": "ADMIN",
      "userType": "HUMAN"
    }
  }
]
```

### Render Barcode
`POST /api/v1/render/barcode`  
Renders a barcode as a Base64 encoded string based on provided properties.

**Request Body**:
```json
{
  "content": "123456789012",
  "type": "EAN13",
  "width": "200",
  "height": "100"
}
```

**Response**:
```text
"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA..."
```

---
