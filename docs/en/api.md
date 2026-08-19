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
    - [Get Unique Code Weights](#get-unique-code-weights)
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
    "ignore_product_out_of_weight_range": true,
    "product_min_weight": 900,
    "product_max_weight": 1100,
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
  "ignore_product_out_of_weight_range": true,
  "product_min_weight": 900,
  "product_max_weight": 1100,
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
- `ignoreProductOutOfWeightRange` (boolean): Enforce the product net weight range on the applicator line.
- `productMinWeight` (int): Minimum allowed product net weight, grams.
- `productMaxWeight` (int): Maximum allowed product net weight, grams.
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
  "ignore_product_out_of_weight_range": true,
  "product_min_weight": 900,
  "product_max_weight": 1100,
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
  "ignore_product_out_of_weight_range": true,
  "product_min_weight": 950,
  "product_max_weight": 1150,
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

#### Get Unique Code Weights
`POST /api/v1/categories/{categoryId}/unique-codes/weight`  
Resolves product IDs, net weights, and tare weights for a given list of unique marking codes.  Unfound codes return null attributes.

**Request Body**:
```json
{
  "codes": [
    "010460123456789021ABC123",
    "010460123456789021XYZ999"
  ]
}
```

**Response**:
```json
{
  "size": 2,
  "code_weight": [
    {
      "code": "010460123456789021ABC123",
      "product_id": 213,
      "net_weight": 100,
      "tare_weight": 20
    },
    {
      "code": "010460123456789021XYZ999",
      "product_id": null,
      "net_weight": null,
      "tare_weight": null
    }
  ]
}
```

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

**Several batches may be active at the same time** (one per production line). In exchange, `name` and `external_id` are globally unique across all batches, closed ones included, and both are required when opening a batch.

### List Batches
`GET /api/v1/product-batches`  
Returns product batches, newest first.
Query Parameters: `active` (optional) — `true` returns only active batches (oldest first), `false` only closed ones, omitted returns all batches.

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

### Get Batch
`GET /api/v1/product-batches/{id}`  
Returns a single batch.

**Errors**: `404` when the batch does not exist.

**Response**:
```json
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
```

### Batch Report by Category
`GET /api/v1/product-batches/{id}/categories`  
Returns the products of the batch aggregated by category, ordered by category name. Weights are in grams. Categories without products in the batch are not reported; products whose category has been deleted are grouped into a single row with a `null` `category_name` and `category_external_id`.

**Errors**: `404` when the batch does not exist.

**Response**:
```json
[
  {
    "category_id": "govyadina",
    "category_name": "Говядина",
    "category_external_id": "EXT-C-01",
    "products_count": 300,
    "products_net_weight": 150000,
    "products_gross_weight": 165000
  }
]
```

### Get Active Batch (deprecated)
`GET /api/v1/product-batches/active`  
Returns the **most recently started** active batch. Responds `404` when no batch is active.

_Deprecated_: use `GET /api/v1/product-batches?active=true` instead. This endpoint is kept for applicator panels that do not yet support batch selection — they keep their last known batch on anything but a `404`, so it must not be removed until every panel is updated.

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
Opens a new product batch. Existing active batches are left untouched.
Query Parameters: `name`, `externalId`. Both are trimmed and must be non-blank and unused.

**Errors** (all `400 Bad Request`): blank `name`; blank `externalId`; `name` already used by another batch; `externalId` already used by another batch.

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
`POST /api/v1/product-batches/{id}/close`  
Closes the given batch: recalculates its statistics, sets `end`, and generates its reports. Returns the closed batch.

**Errors**: `404` when the batch does not exist; `400` when the batch is already closed.

### Close Latest Active Batch (deprecated)
`POST /api/v1/product-batches/close`  
Closes the **most recently started** active batch and returns it. Responds `404` when no batch is active.

_Deprecated_: use `POST /api/v1/product-batches/{id}/close` instead. Kept for admin UI versions deployed independently of the server.

### Batch Report
`GET /api/v1/product-batches/{id}/report`  
Returns a generated CSV report for the batch.
Query Parameters: `type` — `BATCH` for the batch summary, `PRODUCT` for the per-product report.

**Errors**: `400` when the reports have not been generated yet (the batch is still open); `404` when the batch or the report file does not exist.

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

---

## Data Export

Export production data from configured source tables to external databases (PostgreSQL, MySQL, MS SQL Server, Oracle) on a schedule. The set of exportable source tables (or views) is defined by the `export.tables` configuration property; each must have an `id` column used as the watermark. An export task reads rows with `id` greater than the task's watermark (`last_exported_id`) and inserts them into the target table, so each run only transfers new records.

All data export endpoints require the `ADMIN` role.

### List Exportable Tables
`GET /api/v1/export-tables`  
Returns the names of the source tables available for export, as configured in `export.tables`.

**Response**:
```json
["final_product", "product_pack", "product_batch", "product_pallet"]
```

### List Source Table Columns
`GET /api/v1/export-tables/{table}/columns`  
Returns the columns of an exportable source table with their data types, read from the local database via JDBC metadata. The `table` must be one of the names returned by `GET /api/v1/export-tables`. Use this to build column mappings.

**Response**:
```json
[
  {"name": "id", "type": "int8", "category": "NUMERIC"},
  {"name": "net_weight", "type": "int4", "category": "NUMERIC"},
  {"name": "created_at", "type": "timestamp", "category": "DATETIME"}
]
```

`category` is one of `NUMERIC`, `TEXT`, `BOOLEAN`, `DATETIME`, `BINARY`, `OTHER` — a broad grouping of the SQL type for basic type-compatibility checks when mapping columns.

### Export Connections

#### List Connections
`GET /api/v1/export-connections`  
Returns all configured target database connections. Passwords are never returned.

#### Get Connection
`GET /api/v1/export-connections/{id}`

#### Create Connection
`POST /api/v1/export-connections`

**Request Body**:
```json
{
  "name": "warehouse mysql",
  "db_type": "MYSQL",
  "host": "192.168.1.10",
  "port": 3306,
  "database_name": "warehouse",
  "username": "export",
  "password": "secret",
  "properties": "useSSL=false"
}
```

`db_type` is one of `POSTGRESQL`, `MYSQL`, `MSSQL` (MS SQL Server, including Express editions), `ORACLE`. For `ORACLE`, `database_name` is the service name (`jdbc:oracle:thin:@//host:port/service`). `properties` is an optional string of extra JDBC parameters appended to the connection URL.

The connection is verified against the target database before saving: the request fails with `400` if the target is unreachable or the credentials are wrong.

**Response**:
```json
{
  "id": "a1b2c3d4-...",
  "name": "warehouse mysql",
  "db_type": "MYSQL",
  "host": "192.168.1.10",
  "port": 3306,
  "database_name": "warehouse",
  "username": "export",
  "properties": "useSSL=false",
  "created_at": "2026-06-11T10:00:00"
}
```

#### Update Connection
`PUT /api/v1/export-connections/{id}`  
Same body as create. A blank or missing `password` keeps the stored one. Like create, the updated connection is verified against the target database before saving.

#### Delete Connection
`DELETE /api/v1/export-connections/{id}`  
Fails with `400` if the connection is still used by export tasks.

#### Test Connection
`POST /api/v1/export-connections/{id}/test`  
Opens a connection to the target database and validates it.

**Response**:
```json
{
  "success": false,
  "message": "Failed to connect to 'warehouse mysql': Connection refused"
}
```

#### Target Tables
`GET /api/v1/export-connections/{id}/tables`  
Reads the tables of the target database with their columns via JDBC metadata. Limited to the connection's current catalog and schema (e.g. `public` for PostgreSQL, `dbo` for MSSQL, the user's schema for Oracle). Used by the UI to offer target table and target column selection when building an export task.

**Response**:
```json
[
  {
    "name": "products",
    "columns": [
      {"name": "id", "type": "int8", "category": "NUMERIC"},
      {"name": "weight", "type": "numeric", "category": "NUMERIC"}
    ]
  },
  {"name": "orders", "columns": [{"name": "id", "type": "int8", "category": "NUMERIC"}]}
]
```

#### Target Table Columns
`GET /api/v1/export-connections/{id}/tables/{table}/columns`  
Reads the column names of a table in the target database (the table name may be schema-qualified, e.g. `dbo.products`). Useful for building column mappings in the UI.

**Response**:
```json
["id", "weight", "exported_at"]
```

### Export Tasks

#### List Tasks
`GET /api/v1/export-tasks`

#### Get Task
`GET /api/v1/export-tasks/{id}`

#### Create Task
`POST /api/v1/export-tasks`

**Request Body**:
```json
{
  "name": "final products to warehouse",
  "connection_id": "a1b2c3d4-...",
  "source_table": "final_product",
  "target_table": "products",
  "column_mappings": [
    { "source": "id", "target": "id" },
    { "source": "net_weight", "target": "weight" },
    { "source": "created_at", "target": "produced_at" }
  ],
  "cron_expression": "0 0/15 * * * *",
  "enabled": true
}
```

- `source_table`: one of the tables returned by `GET /api/v1/export-tables`. Must have an `id` column.
- `column_mappings`: each `source` must be a column of the source table (validated against the live schema); `target` is the column in the target table. A source column and a target column may each appear in at most one mapping.
- `cron_expression`: Spring 6-field cron (`second minute hour day month weekday`). Triggers fire in the server timezone (`server.timezone`). Optional: `null` or blank means the task is manual-only and never runs on a schedule — it can still be triggered via the run endpoint.
- `enabled`: optional, defaults to `true`. Disabled tasks are not scheduled.

**File targets** (the connection's `target_type` is `FILE`): `target_table` is not used, the `target` of each mapping is the column header, and the following fields apply instead:

- `delimiter`: required, one of `COMMA`, `PIPE`, `TAB`.
- `file_mask`: optional file name prefix; the file name is the mask followed by a `yyMMddHHmmss` timestamp.
- `file_extension`: optional, one of `TXT`, `CSV`, `PSV`, `TSV`. Defaults to the delimiter's extension (`CSV`, `PSV`, `TSV` respectively).
- `start_header`, `end_header`: optional markers written as the first and last cell of the header row. They mark where the header starts and ends and have no data column of their own, so data rows are not padded for them. Neither may contain the delimiter or a line break.
- `header_in_each_row`: optional, defaults to `false`. When `true`, the header row is repeated before every data row.

The last three fields apply to the `TXT` extension only and are ignored for the CSV-family extensions. For example, with `PIPE`, `TXT`, `start_header` `H_S`, `end_header` `H_E`, headers `H1`, `H2`, `H3` and `header_in_each_row` enabled, the exported file is:

```text
H_S|H1|H2|H3|H_E
1|2|3
H_S|H1|H2|H3|H_E
4|5|6
```

**Response**: the created task, including `last_exported_id` (the export watermark, starts at `0`).

#### Update Task
`PUT /api/v1/export-tasks/{id}`  
Same body as create. Changing the source table resets the export watermark to `0`.

#### Delete Task
`DELETE /api/v1/export-tasks/{id}`  
Removes the task from the schedule and deletes its run logs.

#### Run Task Now
`POST /api/v1/export-tasks/{id}/run`  
Triggers an asynchronous run of the task outside of its schedule. If the task is already running, the run is skipped.

#### Task Run Logs
`GET /api/v1/export-tasks/{id}/logs`  
Returns the latest 100 runs of the task, newest first.

**Response**:
```json
[
  {
    "id": 12,
    "task_id": "t1u2v3...",
    "status": "SUCCESS",
    "started_at": "2026-06-11T10:15:00",
    "finished_at": "2026-06-11T10:15:02",
    "rows_exported": 250,
    "from_id": 1000,
    "to_id": 1250,
    "error_message": null
  }
]
```
