![Logo](https://evolvepos.com/wp-content/uploads/2024/08/EvolveLogo.png)
# EvolvePos API

The EvolvePOS API was developed to seamlessly integrate **EvolvePOS** with **WooCommerce**, enabling centralized management of key retail operations. Through this integration, users can manage Departments (Categories), Categories (Subcategories), Products, Images, and Orders directly from the EvolvePOS system.

This API runs locally and is designed to be installed on the client’s server as a **Windows service**. This setup ensures that the API remains accessible even when no user is logged into the Windows environment, providing uninterrupted functionality and reliability.

## ️ Authentication

This API uses **Bearer Token Authentication**. All requests to protected endpoints must include an `Authorization` header with a valid token.

**Header Example:**
```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

### Common Authentication Errors:

An unexpected error occurred.
| Status | Description              |
|--------|-------------------------|
| `401`  | Missing or invalid token. |
| `403`  | Token provided, but lacks permission to access the resource.  |
---

## Documentation for Uploading Items to WooCommerce

The EvolvePos API allows the upload of products to WooCommerce using a JSON format. The following endpoints allow interaction with WooCommerce products, enabling retrieval, creation, updates, and status management based on SKU.


### Get All Products  
**Endpoint:** `GET /woocommerce/products`  
**Description:** Retrieves all products from WooCommerce.  
**Authentication:** Bearer Token (required) 

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | Product status updated successfully. |
| `500`  | An unexpected error occurred.  |
---
### Get Product by SKU
**Endpoint:** `GET /woocommerce/products/{sku}`  
**Description:** Retrieves a specific product from WooCommerce by its SKU. 
**Authentication:** Bearer Token (required) 

**Path Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `sku`     | string | ✅        | Product SKU.       |

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | Product retrieved successfully. |
| `404`  | Product with the specified SKU was not found.  |
| `500`  | An unexpected error occurred.  |

---
### Create or Update Product by SKU
**Endpoint:** `PUT /woocommerce/products/{sku}`  
**Description:** Updates an existing product by SKU, or creates it if it doesn’t exist. 
**Authentication:** Bearer Token (required) 

**Path Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `sku`     | string | ✅        | Product SKU.       |

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | Product retrieved successfully. |
| `404`  | Product with the specified SKU was not found.  |
| `500`  | An unexpected error occurred.  |
---
### Update Product Status
**Endpoint:** `PUT /woocommerce/products/{sku}/status`  
**Description:** Updates the publication status of a product (either`publish` or `draft`).
**Authentication:** Bearer Token (required) 

**Path Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `sku`     | string | ✅        | Product SKU.       |

**Query Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `is_published`     | bolean | ✅       | `true` to publish the product, `false` to set it as a draft.      |

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | Product status update successfully. |
| `500`  | An unexpected error occurred.  |

The structure of an example request for uploading products is described below.

### JSON Structure

The JSON is sent as an array of objects, where each object represents a product to be uploaded to WooCommerce.

#### JSON Fields

| Field | Type | Description | Example |
| :-------- | :------- | :------------------------ |:--- |
| `WebCode` | `string` | Unique code of the product in the store. |"002"|
| `WebDescription` | `string` | Product name. | "TACO CHICO C.SPECIAL"|
| `WebExtendedDescription` | `string` | Extended description of the product, with more details. | "Tacos with tomato and onion and lettuce"|
| `WebDepartment` | `string` | Main category of the product (e.g. "FOOD", "GROCERY"). | "FOOD"|
| `WebSubDepartment` | `string` | Subcategory of the product (can be null if not applicable). | `null`|
| `WebBrand` | `string` | Brand of the product (can be null if not applicable). | `null`|
| `WebPrice` | `float` | Price of the product. | 2.00 |
| `WebManageStock` | `boolean` | Indicates whether the stock of the product is managed manually (true or false). | false |
| `WebStockStatus` | `int` | Stock status. Possible values: 1 (in stock), 0 (out of stock). | 1 |
| `WebStockQuantity` | `int` | Quantity of products in stock. | 0 |
| `WebPromoPrice` | `float` | Promotional price of the product. If there is no promotion, this value is 0. | 0.00 |
| `PromoStartDate` | `string` | Promotion start date (if applicable). In YYYY-MM-DD format or null if there is no promotion. | null |
| `PromoEndDate` | `string` | Promotion end date (if applicable). In YYYY-MM-DD format or null if there is no promotion. | null |
| `WebStatus` | `int` | Product status. Possible values: 1 (active), 0 (inactive). | 0 |

### JSON example

```json
[
 {
 "WebCode": "002",
 "WebDescription": "TACO CHICO C.SPECIAL",
 "WebExtendedDescription": "Web tacos",
 "WebShortDescription": "Soft and Tasty",
 "WebDepartment": "FOOD",
 "WebSubDepartment": null,
 "WebBrand": null,
 "WebPrice": 2,
 "WebManageStock": false,
 "WebStockStatus": 1,
 "WebStockQuantity": 0,
 "WebPromoPrice": 0,
 "PromoStartDate": null,
 "PromoEndDate": null,
 "WebStatus": 0
 },
 {
 "WebCode": "9",
 "WebDescription": "MISC ITEM NO TAX",
 "WebExtendedDescription": "Item No Tax",
 "WebShortDescription": "Item No Tax To",
"WebDepartment": "GROCERY",
"WebSubDepartment": null,
"WebBrand": null,
"WebPrice": 7,
"WebManageStock": false,
"WebStockStatus": 1,
"WebStockQuantity": 0,
"WebPromoPrice": 0,
"PromoStartDate": null,
"PromoEndDate": null,
"WebStatus": 1
}
]

```

### Additional Notes:

- **Stock and Promotions:** If the product has manually managed stock, make sure `WebManageStock` is set to `false`. If a promotional price is offered, you can include the start and end dates for the promotion.
- **Product Status:** The `WebStatus` field defines whether or not the product is active in the store. A value of `1` indicates that the product is active, while `0` disables it.

---

## Image Upload Documentation

The EvolvePos API allows you to associate images with WooCommerce products using the product SKU. These endpoints handle product images and their synchronization with WooCommerce.


###  **Get Product Images by SKU**
**Endpoint:** `GET /product-images/{sku}`  
**Description:** Fetches the product's SKU, name, and associated images from WooCommerce.  
**Authentication:** Bearer Token (required)  

**Path Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `sku`     | string | ✅        | Product SKU.       |

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | Product and images found. |
| `404`  | Product not found.        |
| `500`  | Internal server error.    |

**Response Example:**
```json
{
    "sku": "12345",
    "name": "Soft and Tasty Taco",
    "images": [
        "https://yourstore.com/wp-content/uploads/2025/01/taco1.jpg",
        "https://yourstore.com/wp-content/uploads/2025/01/taco2.jpg"
    ]
}
```
---
###  **Upload All Images from Location Folder**
**Endpoint:** `POST /upload-images`  
**Description:** Uploads all images from the configured `LocationFolder` directories in the database to WordPress. 
**Authentication:** Bearer Token (required)  

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | All images uploaded successfully. |
| `500`  | Internal server error.    |

**Response Example:**
```json
{
    "status": "success",
    "message": "All images have been uploaded successfully."
}
```
---
###  **Sync Gallery Images to Product by SKU**
**Endpoint:** `POST /sync-gallery-images/{sku}`  
**Description:** Syncs a list of images to a product’s gallery by SKU, with an option to replace images at specific indices.
**Authentication:** Bearer Token (required)  

**Path Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `sku`     | string | ✅        | Product SKU.       |

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | Images synced successfully. |
| `500`  | Internal server error.    |

**Response Example:**
```json
{
    "status": "success",
    "uploaded_images": [
        "https://yourstore.com/wp-content/uploads/2025/01/taco1.jpg",
        "https://yourstore.com/wp-content/uploads/2025/01/taco2.jpg"
    ]
}
```


To do so, you send a JSON containing the list of images and their respective positions.

### JSON Structure

The JSON contains a main object with the `images` field, which is an array of objects. Each object within this array represents an image that will be associated with the product.

#### JSON Fields

| Field | Type | Description | Example |
| :-------- | :------- | :------------------------ |:--- |
| `images` | `array` | List of images to associate with the product. |`[ { "path": "...", "index": 0 }, {...} ]`|
| `path` | `string` | Path to the image file. The path must be accessible from the server where WooCommerce is running. | `\\\\192.168.5.10\\red-images\\1.png`|
| `index` | `int` | Image index. Defines the image order for the product. Index 0 is the main image. | `0`,`1`,`2`|

### Detailed Description:

- **`images`:** This is the main field, which contains an array of objects. Each object within images is an image that will be associated with the product. The array can contain multiple images for a single product.
- **`path`:** The full path where the image is located. This must be a valid path on the server. In the example, shared network paths are used (\192.168.5.10...), but you can have local paths or absolute URLs depending on how you manage your images on the server.
- **`index`:** This is the index of the image within the product. Index 0 is considered the main image of the product (the one that will appear as the main one in the store). Subsequent indexes (1, 2, etc.) define the order of the secondary images.

### JSON Example

```json
{
"images": [
{
"path": "\\\\192.168.5.10\\images-red\\1.png",
"index": 0
},
{
"path": "\\\\192.168.5.10\\images-red\\2.jpg",
"index": 1
},
{
"path": "\\\\192.168.5.10\\images-red\\3.png",
"index": 2
}
]
}

```

### API Usage

This JSON is sent to the WooCommerce API to associate the images with the corresponding product. The product SKU will be used to look up the product and then associate the images with that product. The images will be loaded from the path provided in the `path` field.

### Additional Notes:

- **Image Path Format:** Make sure your image paths are correct and accessible by the server where your WooCommerce store is hosted. If you use network paths, make sure they are accessible from the production server.
- **Image Order:** The image index is very important. The product can have multiple images, but the image with `index: 0` will be the primary one, which is the one usually displayed in the product preview in the store.
- **External Images:** If your images are hosted on an external server, make sure the path is a publicly accessible URL.

---
## Documentation: WooCommerce Order JSON Structure

These endpoints handle the interaction between the API and WooCommerce orders.

### Fetch All Orders  
**Endpoint:** `GET /woocommerce/orders`  
**Description:** Fetches all WooCommerce orders with an optional status filter.
**Authentication:** Bearer Token (required)

**Query Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `status`     | string | ❌       | Filter orders by status (optional). Example: `completed`, `processing`, `cancelled`, `on-hold`, `pending`,`failed`, `trash`.|

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | Product status updated successfully. |
| `500`  | An unexpected error occurred.    |

---
### Fetch Order by ID
**Endpoint:** `GET /woocommerce/orders/{order_id}`  
**Description:** Fetches a specific WooCommerce order by its ID with an optional status filter.
**Authentication:** Bearer Token (required)

**Path Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `order_id`     | int | ✅      | FiID of the order to fetch.|

**Query Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `status`     | string | ❌       | Filter orders by status (optional). Example: `completed`, `processing`, `cancelled`, `on-hold`, `pending`,`failed`, `trash`.|

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | Product status updated successfully. |
| `404`  | Order not found. |
| `500`  | An unexpected error occurred.    |
---

### Update Order Status
**Endpoint:** `PUT /woocommerce/orders/{order_id}/status`  
**Description:** Updates the status of a WooCommerce order.
**Authentication:** Bearer Token (required)

**Path Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `order_id`     | int | ✅      | FiID of the order to fetch.|

**Query Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `status`     | string | ✅      | FNew order status. Example: `completed`, `processing`, `cancelled`, `on-hold`, `pending`,`failed`, `trash`.|

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | Order status updated successfully. |
| `500`  | An unexpected error occurred.    |
---


### General Structure

The JSON contains an array called `orders`, where each object represents an order with details about billing, shipping, products, taxes, and payment method.

```json
{
  "orders": [
    {
      "id": 5023,
      "status": "completed",
      "currency": "USD",
      "date_created": "2025-03-05T14:32:21",
      "date_modified": "2025-03-05T14:40:18",
      "discount_total": 10.00,
      "discount_tax": 0.30,
      "shipping_total": 7.50,
      "shipping_tax": 0.45,
      "total": 75.50,
      "total_tax": 3.25,
      "billing": {
        "first_name": "Sophia",
        "last_name": "Williams",
        "company": "EvolvePOS",
        "address_1": "1234 Sunset Blvd",
        "address_2": "Apt 7C",
        "city": "Los Angeles",
        "state": "CA",
        "postcode": "90026",
        "country": "US",
        "email": "sophia.williams@evolvepos.com",
        "phone": "2139876543"
      },
      "web_shipping": {
        "first_name": "Sophia",
        "last_name": "Williams",
        "company": "EvolvePOS",
        "address_1": "1234 Sunset Blvd",
        "address_2": "Apt 7C",
        "city": "Los Angeles",
        "state": "CA",
        "postcode": "90026",
        "country": "US",
        "phone": "2139876543"
      },
      "payment_method": "credit_card",
      "payment_method_title": "Credit Card",
      "transaction_id": "TX9876543210",
      "customer_note": "Please deliver between 10am - 2pm.",
      "date_completed": "2025-03-05T14:40:18",
      "date_paid": "2025-03-05T14:35:25",
      "line_items": [
        {
          "sku": "014738920",
          "name": "EvolvePOS Tablet",
          "quantity": 1,
          "subtotal": 49.99,
          "subtotal_tax": 2.50,
          "total": 49.99,
          "total_tax": 2.50,
          "price": 49.99
        },
        {
          "sku": "021547671",
          "name": "POS Stand for Tablet",
          "quantity": 2,
          "subtotal": 15.98,
          "subtotal_tax": 0.80,
          "total": 15.98,
          "total_tax": 0.80,
          "price": 7.99
        }
      ],
      "tax_lines": [
        {
          "rate_code": "TAX-1",
          "rate_id": 1,
          "label": "Sales Tax",
          "compound": false,
          "tax_total": 3.25,
          "shipping_tax_total": 0.45,
          "rate_percent": 7.25
        }
      ],
      "shipping_lines": [
        {
          "method_title": "Standard Shipping",
          "method_id": "standard_shipping",
          "total": 7.50,
          "total_tax": 0.45
        }
      ],
      "fee_lines": [],
      "coupon_lines": [],
      "refunds": []
    }
  ]
}
```

---

### Main Order Fields

- **`id`** (`int`): Unique identifier for the order.  
- **`status`** (`string`): Current status of the order (e.g., `"completed"`).  
- **`currency`** (`string`): Currency used for the order (e.g., `"USD"`).  
- **`date_created`** (`string`): Date and time when the order was created (ISO 8601 format).  
- **`date_modified`** (`string`): Date and time when the order was last modified.  
- **`discount_total`** (`float`): Total amount of discounts applied to the order.  
- **`discount_tax`** (`float`): Tax amount applied to the discount.  
- **`shipping_total`** (`float`): Total shipping cost.  
- **`shipping_tax`** (`float`): Tax amount applied to the shipping cost.  
- **`total`** (`float`): Final total amount for the order.  
- **`total_tax`** (`float`): Total tax amount applied to the order.  

---

### Billing Information (`billing`)

Customer’s billing details:

- **`first_name`** (`string`): Customer's first name.  
- **`last_name`** (`string`): Customer's last name.  
- **`company`** (`string`): Company name (if applicable).  
- **`address_1`** (`string`): Primary address line.  
- **`address_2`** (`string`): Secondary address line (optional).  
- **`city`** (`string`): City of residence.  
- **`state`** (`string`): State or province.  
- **`postcode`** (`string`): Postal or ZIP code.  
- **`country`** (`string`): Country code (ISO 3166-1 alpha-2 format).  
- **`email`** (`string`): Customer’s email address.  
- **`phone`** (`string`): Customer’s phone number.  

---

### Shipping Information (`web_shipping`)

Shipping address for the order, containing the same fields as `billing`.

---

### Payment Information

- **`payment_method`** (`string`): Payment method used (`"credit_card"`, `"paypal"`, etc.).  
- **`payment_method_title`** (`string`): Display name of the payment method.  
- **`transaction_id`** (`string`): Unique identifier for the payment transaction.  
- **`date_paid`** (`string`): Date and time when the payment was completed.  
- **`date_completed`** (`string`): Date and time when the order was marked as completed.  
- **`customer_note`** (`string`): Optional note from the customer.  

---

### Ordered Products (`line_items`)

List of purchased products, each with the following fields:

- **`sku`** (`string`): Product’s unique stock-keeping unit (SKU).  
- **`name`** (`string`): Product name.  
- **`quantity`** (`int`): Quantity purchased.  
- **`subtotal`** (`float`): Price before taxes for the given quantity.  
- **`subtotal_tax`** (`float`): Tax amount applied to the subtotal.  
- **`total`** (`float`): Final price after discounts or adjustments.  
- **`total_tax`** (`float`): Total tax amount for the product.  
- **`price`** (`float`): Unit price of the product.  

**Example:**  
```json
{
  "sku": "014738920",
  "name": "EvolvePOS Tablet",
  "quantity": 1,
  "subtotal": 49.99,
  "subtotal_tax": 2.50,
  "total": 49.99,
  "total_tax": 2.50,
  "price": 49.99
}
```
### Tax Information (`tax_lines`)

List of taxes applied to the order:

- **`rate_code`** (`string`): Tax rate code.  
- **`rate_id`** (`int`): Tax rate identifier.  
- **`label`** (`string`): Tax name (e.g., `"Sales Tax"`).  
- **`compound`** (`bool`): Indicates whether the tax is compounded.  
- **`tax_total`** (`float`): Total tax amount for the products. 
- **`shipping_tax_total`** (`float`): Tax amount applied to shipping. 
- **`rate_percent`** (`float`): Percentage of the tax rate.

**Example:**  
```json
{
  "rate_code": "TAX-1",
  "rate_id": 1,
  "label": "Sales Tax",
  "compound": false,
  "tax_total": 3.25,
  "shipping_tax_total": 0.45,
  "rate_percent": 7.25
}
```
### Shipping Methods (`shipping_lines`)

List of shipping methods used:

- **`method_title`** (`string`): Name of the shipping method (e.g., `"Standard Shipping"`). 
- **`method_id`** (`string`): Identifier of the shipping method.  
- **`total`** (`float`): Total shipping cost. 
- **`total_tax`** (`float`): Tax amount applied to the shipping cost.

**Example:**  
```json
{
  "method_title": "Standard Shipping",
  "method_id": "standard_shipping",
  "total": 7.50,
  "total_tax": 0.45
}
```
### Additional Fields

- **`fee_lines`** (`array`): List of additional fees applied to the order (empty in this case).
- **`coupon_lines`** (`array`): List of discount coupons used (empty in this case).
- **`refunds`** (`array`): List of processed refunds (empty in this case).

### Complete Example

```json
{
  "orders": [
    {
      "id": 5023,
      "status": "completed",
      "currency": "USD",
      "date_created": "2025-03-05T14:32:21",
      "total": 75.50,
      "billing": {
        "first_name": "Sophia",
        "last_name": "Williams",
        "email": "sophia.williams@evolvepos.com"
      },
      "line_items": [
        {
          "sku": "014738920",
          "name": "EvolvePOS Tablet",
          "quantity": 1,
          "total": 49.99
        }
      ],
      "shipping_lines": [
        {
          "method_title": "Standard Shipping",
          "total": 7.50
        }
      ]
    }
  ]
}
```

---
### Fetch Order Notes
**Endpoint:** `GET /woocommerce/orders/{order_id}/notes`  
**Description:** Fetches all notes for a specific WooCommerce order.
**Authentication:** Bearer Token (required)

**Path Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `order_id`     | int | ✅      | ID of the order. |

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | List of order notes. |
| `500`  | An unexpected error occurred.    |

**Example Response:**
```json
{
    "notes": [
        {
            "id": 1,
            "author": "Admin",
            "date_created": "2025-03-10T12:00:00Z",
            "note": "Order shipped.",
            "customer_note": false
        },
        {
            "id": 2,
            "author": "Customer",
            "date_created": "2025-03-11T14:30:00Z",
            "note": "Please deliver after 5 PM.",
            "customer_note": true
        }
    ]
}
```
---
### Add Order Note
**Endpoint:** `POST /woocommerce/orders/{order_id}/notes`  
**Description:** Adds a note to a WooCommerce order.
**Authentication:** Bearer Token (required)

**Path Parameters:**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `order_id`     | int | ✅      | ID of the order. |

**Request Body**
| Parameter | Type   | Required | Description       |
|-----------|--------|----------|-------------------|
| `note`     | string | ✅      | The content of the note. |
| `visible`     | boolean | ❌     | Whether the note is visible to the customer (default: `true`). |

**Responses:**
| Status | Description              |
|--------|-------------------------|
| `200`  | 	Note added successfully. |
| `500`  | An unexpected error occurred.    |

**Example Request:**
```json
{
    "note": "Your order is ready for pickup.",
    "visible": true
}
```

These endpoints are designed to help keep your order data synchronized and up-to-date across platforms.
