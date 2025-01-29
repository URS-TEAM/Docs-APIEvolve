![Logo](https://evolvepos.com/wp-content/uploads/2024/08/EvolveLogo.png)
# EvolvePos API

## Documentation for Uploading Items to WooCommerce

The EvolvePos API allows the upload of products to WooCommerce using a JSON format. The structure of an example request for uploading products is described below.

## JSON Structure

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

## JSON example

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

## Additional Notes:

- **Stock and Promotions:** If the product has manually managed stock, make sure `WebManageStock` is set to `false`. If a promotional price is offered, you can include the start and end dates for the promotion.
- **Product Status:** The `WebStatus` field defines whether or not the product is active in the store. A value of `1` indicates that the product is active, while `0` disables it.

## Image Upload Documentation

The EvolvePos API allows you to associate images with WooCommerce products using the product SKU. To do so, you send a JSON containing the list of images and their respective positions.

## JSON Structure

The JSON contains a main object with the `images` field, which is an array of objects. Each object within this array represents an image that will be associated with the product.

#### JSON Fields

| Field | Type | Description | Example |
| :-------- | :------- | :------------------------ |:--- |
| `images` | `array` | List of images to associate with the product. |`[ { "path": "...", "index": 0 }, {...} ]`|
| `path` | `string` | Path to the image file. The path must be accessible from the server where WooCommerce is running. | `\\\\192.168.5.10\\red-images\\1.png`|
| `index` | `int` | Image index. Defines the image order for the product. Index 0 is the main image. | `0`,`1`,`2`|

## Detailed Description:

- **`images`:** This is the main field, which contains an array of objects. Each object within images is an image that will be associated with the product. The array can contain multiple images for a single product.
- **`path`:** The full path where the image is located. This must be a valid path on the server. In the example, shared network paths are used (\192.168.5.10...), but you can have local paths or absolute URLs depending on how you manage your images on the server.
- **`index`:** This is the index of the image within the product. Index 0 is considered the main image of the product (the one that will appear as the main one in the store). Subsequent indexes (1, 2, etc.) define the order of the secondary images.

## JSON Example

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

## API Usage

This JSON is sent to the WooCommerce API to associate the images with the corresponding product. The product SKU will be used to look up the product and then associate the images with that product. The images will be loaded from the path provided in the `path` field.

## Additional Notes:

- **Image Path Format:** Make sure your image paths are correct and accessible by the server where your WooCommerce store is hosted. If you use network paths, make sure they are accessible from the production server.
- **Image Order:** The image index is very important. The product can have multiple images, but the image with `index: 0` will be the primary one, which is the one usually displayed in the product preview in the store.
- **External Images:** If your images are hosted on an external server, make sure the path is a publicly accessible URL.
