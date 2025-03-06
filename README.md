# Authentication API Documentation

## Overview
This document provides details about the `POST /login` endpoint, including request payload, response structure, and usage.

## Endpoint
```
{{baseurl}}/login
```

## Method
**POST**

## Request Payload
| Parameter  | Type   | Required | Description                        |
|------------|--------|----------|------------------------------------|
| `email`    | String | Yes      | The user's email address.         |
| `password` | String | Yes      | The user's password.              |

### Example Request
```json
{
  "email": "superadmin@gmail.com",
  "password": "superadmin"
}
```

## Response
| Parameter  | Type   | Description                           |
|------------|--------|--------------------------------------|
| `message`  | String | Response message indicating success. |
| `token`    | String | Authentication token for the session. |

### Example Response
```json
{
    "message": "Login successful",
    "token": "22|lqKdllGp9EwAHXPdcDqLwq9P3vASZfJ7pWxrvfn987345499"
}
```

## Notes
- The `token` should be used for authentication in subsequent requests.
- Ensure HTTPS is used for secure transmission.
- Implement proper security measures, such as hashing passwords and rate-limiting login attempts.

# Budget Creation API Documentation

## Overview
This document provides details about the `POST /budget/create` endpoint, including request payload, response structure, and usage.

## Endpoint
```
{{baseurl}}/budget/create
```

## Method
**POST**

## Request Payload
| Parameter           | Type     | Required | Description |
|---------------------|----------|----------|-------------|
| `name`             | String   | Yes      | Name of the budget project. |
| `product_id`       | String   | Yes      | ID of the product associated with the budget. |
| `main`             | Object   | Yes      | Contains details of the main product dimensions and finishing. |
| `main.[index].h`   | Integer  | Yes      | Height of the product in mm. |
| `main.[index].w`   | Integer  | Yes      | Width of the product in mm. |
| `main.[index].id`  | Integer  | Yes      | ID of the product type. |
| `main.[index].thickness` | Integer | Yes  | Thickness of the product in mm. |
| `main.[index].finishing` | String | Yes | Finishing type of the product. |
| `main.[index].sub.id` | Integer | Yes | Sub-component ID. |
| `main.[index].sub.size` | Integer | Yes | Sub-component size in mm. |
| `topCuts`          | Object   | No       | Contains cut details. |
| `topCuts.[index].width`  | String  | Yes | Width of the cut in mm. |
| `topCuts.[index].height` | String  | Yes | Height of the cut in mm. |
| `topCuts.[index].quantity` | Integer | Yes | Quantity of cuts. |
| `service_type`     | String   | Yes      | Type of service (e.g., delivery only). |
| `zip`             | String   | Yes      | Zip code related to the service. |
| `reseller_margin` | String   | Yes      | Reseller margin percentage. |

### Example Request
```json
{
  "name": "project name 1",
  "product_id": "75",
  "main": {
    "1": {
      "h": 900,
      "w": 2000,
      "id": 494,
      "thickness": 20,
      "finishing": "Anticato",
      "sub": {
        "id": 17,
        "size": 900
      }
    },
    "2": {
      "w": 2000,
      "h": 1000,
      "id": 494,
      "thickness": 20,
      "finishing": "Anticato",
      "sub": {
        "id": 13,
        "size": 3000
      }
    }
  },
  "topCuts": {
    "51": {
      "width": "900",
      "height": "490",
      "quantity": 1
    },
    "53": {
      "width": "560",
      "height": "490",
      "quantity": 2
    }
  },
  "service_type": "delivery only",
  "zip": "12",
  "reseller_margin": "0"
}
```

## Response
| Parameter            | Type    | Description |
|----------------------|---------|-------------|
| `success`           | String  | Indicates successful budget creation. |
| `id`               | Integer | ID of the newly created budget. |
| `pdf`              | String  | URL to the generated PDF document. |
| `code`             | Integer | HTTP status code. |
| `data`             | Object  | Contains detailed budget data. |
| `data.plates`      | Object  | Plate details including brand, product, and pricing. |
| `data.edges`       | Object  | Edge finishing details including name, size, and price. |
| `data.surfaces`    | Object  | Surface processing details and price. |
| `data.service_type` | String | Type of service requested. |
| `data.service_charge` | String | Additional charge for the service. |
| `data.final_price` | String | Total budget price. |

### Example Response
```json
{
    "success": "successfully created",
    "id": 1093,
    "pdf": "http://gramafam.test/admin/budgets/1/pdf/1093",
    "code": 200,
    "data": {
        "plates": {
            "494": {
                "brand": "Granito",
                "product": "Granito Preto Zimbabwe Anticato",
                "variation": "Granito Preto Zimbabwe - Anticato",
                "finishing": "Anticato",
                "size": "20mm 3x1.9m",
                "qtd": 1,
                "unid": "3.80m",
                "qtd_comprada_per_m2": "5.70",
                "purchase_price_per_m2": "104.50",
                "purchase_price": "595.65",
                "selling_price": "1,548.69",
                "pieces": [
                    {
                        "size": "2x1",
                        "qty": 1,
                        "size_sq": "2.00m"
                    },
                    {
                        "size": "2x0.9",
                        "qty": 1,
                        "size_sq": "1.80m"
                    }
                ]
            }
        },
        "service_type": "delivery only",
        "service_charge": "43.00",
        "final_price": "1,840.19"
    }
}
```

## Notes
- Ensure valid `product_id` exists in the database before making requests.
- The `pdf` URL can be used to download the generated budget document.
- Prices are calculated based on product dimensions and finishing.

# GET /products API Documentation

## Overview
This document provides details about the `GET /products` endpoint, including query parameters and their descriptions.

## Endpoint
```
{{baseurl}}/products
```

## Method
**GET**

## Query Parameters
| Parameter             | Type    | Required | Description |
|----------------------|---------|----------|-------------|
| `product_category_id` | Integer | No       | The ID of the product category to filter products. |
| `product_brand_id`    | Integer | No       | The ID of the product brand to filter products. |
| `title`              | String  | No       | The title or name of the product to search for. |
| `page`               | Integer | No       | The page number for paginated results. |

## Example Request
```
GET {{baseurl}}/products?product_category_id=2&product_brand_id=4&title=Blanco City&page=3
```

---

#### Response Format

```json
{
    "current_page": 1,
    "data": [
        {
            "id": 1,
            "title": "Blanco City",
            "description": "<p>Blanco City, material do grupo 1...</p>",
            "product_category_id": 2,
            "product_brand_id": 4,
            "image": "uploaded_files/products/6505e590782ee396.jfif",
            "margin": 20
        }
    ],
    "first_page_url": "http://example.com/api/products?page=1",
    "from": 1,
    "last_page": 25,
    "last_page_url": "http://example.com/api/products?page=25",
    "links": [
        {
            "url": null,
            "label": "pagination.previous",
            "active": false
        },
        {
            "url": "http://example.com/api/products?page=1",
            "label": "1",
            "active": true
        }
    ],
    "next_page_url": "http://example.com/api/products?page=2",
    "path": "http://example.com/api/products",
    "per_page": 20,
    "prev_page_url": null,
    "to": 20,
    "total": 500
}
```

#### Response Fields

| Field                 | Type    | Description |
|----------------------|---------|-------------|
| `current_page`       | int     | Current page number. |
| `data`               | array   | List of products matching the criteria. |
| `first_page_url`     | string  | URL of the first page. |
| `from`               | int     | Starting index of items on this page. |
| `last_page`          | int     | Total number of pages. |
| `last_page_url`      | string  | URL of the last page. |
| `links`              | array   | Pagination links. |
| `next_page_url`      | string  | URL of the next page, if available. |
| `path`               | string  | Base path of the API endpoint. |
| `per_page`           | int     | Number of items per page. |
| `prev_page_url`      | string  | URL of the previous page, if available. |
| `to`                 | int     | Ending index of items on this page. |
| `total`              | int     | Total number of products available. |

#### Product Object Fields

| Field                 | Type    | Description |
|----------------------|---------|-------------|
| `id`                | int     | Unique identifier of the product. |
| `title`             | string  | Name of the product. |
| `description`       | string  | Product description in HTML format. |
| `product_category_id` | int   | Category ID the product belongs to. |
| `product_brand_id`   | int    | Brand ID of the product. |
| `image`             | string  | Image URL of the product. |
| `margin`            | int     | Margin percentage for the product. |

#### Notes
- All string values are UTF-8 encoded.
- Pagination follows standard RESTful API best practices.
- Filtering parameters are optional; omitting them returns all available products.

# Zip Codes API Documentation

## Overview
This document provides details about the `GET /zipcodes` endpoint, including request parameters, response structure, and usage.

## Endpoint
```
{{baseurl}}/zipcodes?zip_code=4100&page=2
```

## Method
**GET**

## Query Parameters
| Parameter   | Type    | Required | Description |
|------------|---------|----------|-------------|
| `zip_code` | String  | Yes      | The postal code to search for. |
| `page`     | Integer | No       | The page number for paginated results. |

## Example Request
```
GET {{baseurl}}/zipcodes?zip_code=4100&page=2
```

## Response

| Parameter      | Type    | Description |
|---------------|---------|-------------|
| `current_page` | Integer | Current page number. |
| `data`        | Array   | List of zip code entries. |
| `data[].id`   | Integer | Unique identifier for the zip code. |
| `data[].zip_code` | String | The zip code value. |
| `first_page_url` | String | URL of the first page. |
| `last_page`   | Integer | Total number of pages. |
| `last_page_url` | String | URL of the last page. |
| `links`       | Array   | Pagination links. |
| `links[].url` | String | URL for pagination navigation. |
| `links[].label` | String | Label for pagination (e.g., previous, next, page number). |
| `links[].active` | Boolean | Indicates if the pagination link is active. |
| `next_page_url` | String | URL for the next page (if applicable). |
| `path`        | String | API base path for pagination. |
| `per_page`    | Integer | Number of results per page. |
| `prev_page_url` | String | URL for the previous page (if applicable). |
| `total`       | Integer | Total number of results available. |

### Example Response
```json
{
    "current_page": 2,
    "data": [
        { "id": 10, "zip_code": "1000" },
        { "id": 11, "zip_code": "2750" },
        { "id": 13, "zip_code": "3060" }
    ],
    "first_page_url": "http://gramafam.test/api/zipcodes?page=1",
    "last_page": 1,
    "last_page_url": "http://gramafam.test/api/zipcodes?page=1",
    "links": [
        { "url": "http://gramafam.test/api/zipcodes?page=1", "label": "pagination.previous", "active": false },
        { "url": "http://gramafam.test/api/zipcodes?page=1", "label": "1", "active": false },
        { "url": null, "label": "pagination.next", "active": false }
    ],
    "next_page_url": null,
    "path": "http://gramafam.test/api/zipcodes",
    "per_page": 20,
    "prev_page_url": "http://gramafam.test/api/zipcodes?page=1",
    "total": 1
}
```

## Notes
- The `zip_code` parameter must be a valid postal code.
- Pagination links provide navigation between pages.
- The response includes multiple zip codes based on the query.
