# Pharma-Warehouse API Documentation

## Introduction

This document outlines the functionalities and usage of the Pharma-Warehouse APIs. It serves as a reference for front-end developers integrating with the backend system.

### Authentication Endpoints

#### 1. Register 
- **URL:**  [http://127.0.0.1:8000/api/register]() 
- **Method:**  POST 
- **Authorization:**  Not required 
- **Parameters:** 
- ```role``` (string, required): User role.
- ```name``` (string, required): User name.
- ```phone``` (numeric, required): User phone number (must be unique).
- ```password``` (string, required): User password (min length: 6, confirmed). 
- **Request Example:** 

```json
{
  "role": "warehouse_owner",
  "name": "John",
  "phone": 1234567890,
  "password": "password123",
  "password_confirmation": "password123"
}
``` 
- **Response Example (Success):** 

```json
{
  "message": "User successfully registered",
  "user": {
    "id": 1,
    "name": "John",
    "role": "warehouse_owner",
    "phone": 1234567890,
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
  }
}
```

 
- **Status Code:**  `201` Created 

- **Response Example (Error):** 

```json
{
  "error": "Validation error",
  "message": {
    "phone": ["The phone has already been taken."]
  }
}
```

 
- **Status Code:**  `400` Bad Request


#### 2. Login 
- **URL:**  [http://127.0.0.1:8000/api/login]() 
- **Method:**  POST 
- **Authorization:**  Not required 
- **Parameters:** 
- ```phone``` (required): User phone number.
- ```password``` (required): User password (min length: 6). 
- **Request Example:** 

```json
{
  "phone": 1234567890,
  "password": "password123"
}
``` 
- **Response Example (Success):** 

```json
{
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9...",
  "token_type": "bearer",
  "expires_in": 3600,
  "user": {
    "id": 1,
    "name": "John",
    "role": "warehouse_owner",
    "phone": 1234567890,
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
  }
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "error": "Unauthorized",
  "message": "Invalid credentials."
}
```

 
- **Status Code:**  `401` Unauthorized

#### 3. Logout 
- **URL:**  [http://127.0.0.1:8000/api/logout]() 
- **Method:**  POST 
- **Authorization:**  Required (Bearer Token) 
- **Parameters:**  None 
- **Request Example:** 
- No additional parameters are required. 
- **Response Example (Success):** 

```json
{
  "message": "User successfully signed out"
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "error": "Unauthorized",
  "message": "Invalid token"
}
```

 
- **Status Code:**  `401` Unauthorized


### Medication Endpoints


#### 1. Add Medication 
- **URL:**  [http://127.0.0.1:8000/api/admin/medication/add]() 
- **Method:**  POST 
- **Authorization:**  Required (Bearer Token and Warehouse Owner role) 
- **Parameters:** 
- ```scientific_name``` (string, required): Scientific name of the medication.
- ```brand_name``` (string, required): Brand name of the medication.
- ```classification``` (string, required): Classification of the medication.
- ```manufacturer``` (string, optional): Manufacturer of the medication.
- ```available_quantity``` (integer, required): Available quantity of the medication.
- ```expiry_date``` (date, required): Expiry date of the medication (format: Y-m-d).
- ```price``` (float, required): Price of the medication. 
- **Request Example:** 

```json
{
  "scientific_name": "Ibuprofen",
  "brand_name": "Advil",
  "classification": "Pain",
  "manufacturer": "Pfizer",
  "available_quantity": 100,
  "expiry_date": "2024-12-31",
  "price": 9.99
}
``` 
- **Response Example (Success):** 

```json
{
  "message": "Medication added successfully"
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "message": "Validation error",
  "errors": {
    "classification": ["The classification field is required."]
  }
}
```

 
- **Status Code:**  `422` Unprocessable Entity


#### 2. Update Medication 
- **URL:**  [http://127.0.0.1:8000/api/admin/medication/update/{id}]() 
- **Method:**  PUT 
- **Authorization:**  Required (Bearer Token and Warehouse Owner role) 
- **Parameters:** 
- Same as the "Add Medication" endpoint. 
- **Request Example:** 

```json
{
  "scientific_name": "Ibuprofen",
  "brand_name": "Advil",
  "classification": "Pain",
  "manufacturer": "Pfizer",
  "available_quantity": 150,
  "expiry_date": "2025-12-31",
  "price": 9.99
}
``` 
- **Response Example (Success):** 

```json
{
  "message": "Medication updated successfully"
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "message": "Medication not found"
}
```

 
- **Status Code:**  `404` Not Found


#### 3. Delete Medication 
- **URL:**  [http://127.0.0.1:8000/api/admin/medication/delete/{id}]() 
- **Method:**  DELETE 
- **Authorization:**  Required (Bearer Token and Warehouse Owner role) 
- **Parameters:** 
- None 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "message": "Medication deleted successfully"
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "message": "Medication not found"
}
```

 
- **Status Code:**  `404` Not Found


#### 4. List All Medications 
- **URL:**  [http://127.0.0.1:8000/api/medications]() 
- **Method:**  GET 
- **Authorization:**  Not required 
- **Parameters:** 
- None 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "medications": [
    {
      "id": 1,
      "scientific_name": "Ibuprofen",
      "brand_name": "Advil",
      "classification": "Pain",
      "manufacturer": "Pfizer",
      "available_quantity": 150,
      "expiry_date": "2025-12-31",
      "price": 9.99,
      "created_at": "2024-01-01T00:00:00Z",
      "updated_at": "2024-01-01T00:00:00Z"
    },
    // ... other medications
  ]
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 
- No specific error response for this endpoint.


#### 5. Get Medication Details 
- **URL:**  [http://127.0.0.1:8000/api/medications/details/{id}]() 
- **Method:**  GET 
- **Authorization:**  Not required 
- **Parameters:** 
- None 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "medication": {
    "id": 1,
    "scientific_name": "Ibuprofen",
    "brand_name": "Advil",
    "classification": "Pain",
    "manufacturer": "Pfizer",
    "available_quantity": 150,
    "expiry_date": "2025-12-31",
    "price": 9.99,
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
  }
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "message": "Medication not found"
}
```

 
- **Status Code:**  `404` Not Found


#### 6. List Medications by Classification 
- **URL:**  [http://127.0.0.1:8000/api/medications/{classification}]() 
- **Method:**  GET 
- **Authorization:**  Not required 
- **Parameters:** 
- classification (string, required): Classification of medications. 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "medications": [
    {
      "id": 1,
      "scientific_name": "Ibuprofen",
      "brand_name": "Advil",
      "classification": "Pain",
      "manufacturer": "Pfizer",
      "available_quantity": 150,
      "expiry_date": "2025-12-31",
      "price": 9.99,
      "created_at": "2024-01-01T00:00:00Z",
      "updated_at": "2024-01-01T00:00:00Z"
    },
    // ... other medications with the specified classification
  ]
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "message": "Classification parameter is required"
}
```

 
- **Status Code:**  `400` Bad Request


#### 7. Search Medications 
- **URL:**  [http://127.0.0.1:8000/api/medications/search]() 
- **Method:**  POST 
- **Authorization:**  Not required 
- **Parameters:** 
- searchTerm (string, required): Search term for medications. 
- **Request Example:** 
- No additional parameters are required. 
- **Response Example (Success):** 

```json
{
  "medications": [
    {
      "id": 1,
      "scientific_name": "Ibuprofen",
      "brand_name": "Advil",
      "classification": "Pain",
      "manufacturer": "Pfizer",
      "available_quantity": 150,
      "expiry_date": "2025-12-31",
      "price": 9.99,
      "created_at": "2024-01-01T00:00:00Z",
      "updated_at": "2024-01-01T00:00:00Z"
    },
    // ... other medications matching the search term
  ]
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "message": "Medication not found"
}
```

 
- **Status Code:**  `404` Not Found



### Favorite Endpoints


#### 1. Add Medication to Favorites 
- **URL:**  [http://127.0.0.1:8000/api/favorites/add]() 
- **Method:**  POST 
- **Authorization:**  Required (Bearer Token) 
- **Parameters:** 
- ```medication_id``` (integer, required): ID of the medication to be added to favorites. 
- **Request Example:** 

```json
{
  "medication_id": 1
}
``` 
- **Response Example (Success):** 

```json
{
  "message": "Medication added to favorites successfully"
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "error": "Medication already in favorites"
}
```

 
- **Status Code:**  `422` Unprocessable Entity

#### 2. Remove Medication from Favorites 
- **URL:**  [http://127.0.0.1:8000/api/favorites/delete]() 
- **Method:**  DELETE 
- **Authorization:**  Required (Bearer Token) 
- **Parameters:** 
- ```medication_id``` (integer, required): ID of the medication to be removed from favorites. 
- **Request Example:** 

```json
{
  "medication_id": 1
}
``` 
- **Response Example (Success):** 

```json
{
  "message": "Medication removed from favorites successfully"
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "error": "Medication not found in favorites"
}
```

 
- **Status Code:**  `404` Not Found


#### 3. Get Pharmacist's Favorite Medications 
- **URL:**  [http://127.0.0.1:8000/api/favorites]() 
- **Method:**  GET 
- **Authorization:**  Required (Bearer Token) 
- **Parameters:** 
- None 
- **Request Example:** 
- No additional parameters are required. 
- **Response Example (Success):** 

```json
{
  "favorites": [
    {
      "medication_id": 1,
      "medication": {
        "id": 1,
        "scientific_name": "Ibuprofen"
      }
    },
    // ... other favorite medications
  ]
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "error": "You have not added any medications to your favorites yet"
}
```

 
- **Status Code:**  `404` Not Found



### Order Endpoints


#### 1. Place an Order 
- **URL:**  [http://127.0.0.1:8000/api/orders/place]() 
- **Method:**  POST 
- **Authorization:**  Required (Bearer Token) 
- **Parameters:**  
- medications (array, required):
- medication (string, required): Scientific name or ID of the medication.
- quantity (integer, required): Quantity of the medication. 
- **Request Example:** 

```json
{
  "medications": [
    {
      "medication": "Acetaminophen",
      "quantity": 10
    },
    {
      "medication": "2", // Medication ID
      "quantity": 5
    }
  ]
}
``` 
- **Response Example (Success):** 

```json
{
  "message": "Order placed successfully"
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "error": "Medication not found"
}
```

 
- **Status Code:**  `404` Not Found


#### 2. Get Order Details 
- **URL:**  [http://127.0.0.1:8000/api/orders/{id}]() 
- **Method:**  GET 
- **Authorization:**  Not Required
- **Parameters:** 
- None 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "order": {
    "id": 1,
    "pharmacist_id": 2,
    "status": "preparing",
    "payment_status": "unpaid",
    "created_at": "2024-01-03T21:50:32.000000Z",
    "updated_at": "2024-01-03T21:59:19.000000Z",
    "order_medications": [
      {
        "id": 1,
        "order_id": 1,
        "medication_id": 1,
        "quantity": 10,
        "created_at": "2024-01-03T21:50:32.000000Z",
        "updated_at": "2024-01-03T21:50:32.000000Z"
      },
      {
        "id": 2,
        "order_id": 1,
        "medication_id": 2,
        "quantity": 5,
        "created_at": "2024-01-03T21:50:32.000000Z",
        "updated_at": "2024-01-03T21:50:32.000000Z"
      }
    ]
  }
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "error": "Order not found"
}
```

 
- **Status Code:**  `404` Not Found


#### 3. Get Pharmacist's Orders 
- **URL:**  [http://127.0.0.1:8000/api/pharmacist/my-orders]() 
- **Method:**  GET 
- **Authorization:**  Required (Bearer Token) 
- **Parameters:** 
- None 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "orders": [
    {
      "order_id": 1,
      "status": "preparing",
      "payment_status": "unpaid",
      "medications": [
        {
          "medication_id": 2,
          "name": "Paracetamol",
          "quantity": 5
        },
        // ... other medications
      ]
    },
    // ... other orders
  ]
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "error": "You haven't placed any orders yet."
}
```

 
- **Status Code:**  `400` Bad Request


#### 4. View All Orders 
- **URL:**  [http://127.0.0.1:8000/api/orders/all]() 
- **Method:**  GET 
- **Authorization:**  Required (Bearer Token and Warehouse Owner role) 
- **Parameters:** 
- None 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "orders": [
    {
      "id": 1,
      "pharmacist_id": 123,
      "status": "preparing",
      "payment_status": "unpaid",
      "orderMedications": [
        {
          "id": 1,
          "order_id": 1,
          "medication_id": 2,
          "quantity": 5,
          "created_at": "2024-01-01T00:00:00Z",
          "updated_at": "2024-01-01T00:00:00Z"
        },
        // ... other order medications
      ],
      "created_at": "2024-01-01T00:00:00Z",
      "updated_at": "2024-01-01T00:00:00Z"
    },
    // ... other orders
  ]
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "message": "No orders found"
}
```

 
- **Status Code:**  `404` Not Found


#### 5. Change Order Status 
- **URL:**  [http://127.0.0.1:8000/api/orders/change-status/{orderId}/{status}]() 
- **Method:**  PUT 
- **Authorization:**  Required (Bearer Token and Warehouse Owner role) 
- **Parameters:** 
- orderId (integer, path, required): ID of the order.
- status (string, path, required): New status for the order. 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "message": "Order status changed successfully. New quantities: Acetaminophen: 90, Ibuprofen: 75"
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "error": "Order not found"
}
```

 
- **Status Code:**  `404` Not Found


#### 6. Change Payment Status 
- **URL:**  [http://127.0.0.1:8000/api/orders/change-payment-status/{orderId}/{paymentStatus}]() 
- **Method:**  PUT 
- **Authorization:**  Required (Bearer Token and Warehouse Owner role) 
- **Parameters:** 
- orderId (integer, path, required): ID of the order.
- paymentStatus (string, path, required): New payment status for the order. 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "message": "Payment status changed successfully"
}
```

 
- **Status Code:**  `200` OK 

- **Response Example (Error):** 

```json
{
  "error": "Order not found"
}
```

 
- **Status Code:**  `404` Not Found


### Reports Endpoints


#### 1. Daily Sales Report 
- **URL:**  [http://127.0.0.1:8000/api/reports/daily-sales]() 
- **Method:**  GET 
- **Authorization:**  Required (Bearer Token and Warehouse Owner role) 
- **Parameters:** 
- None 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "report_type": "Daily Sales Report",
  "number_of_orders": 1,
  "number_of_pharmacists": 1,
  "quantities_sold": 15,
  "total_prices": 99.85000000000001,
  "best_selling_medication": "Acetaminophen",
  "start_date": "2024-01-03",
  "end_date": "2024-01-03"
}
```

 
- **Status Code:**  `200` OK



#### 2. Weekly Sales Report 
- **URL:**  [http://127.0.0.1:8000/api/reports/weekly-sales]() 
- **Method:**  GET 
- **Authorization:**  Required (Bearer Token and Warehouse Owner role) 
- **Parameters:** 
- None 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "report_type": "Weekly Sales Report",
  "number_of_orders": 15,
  "number_of_pharmacists": 7,
  "quantities_sold": 120,
  "total_prices": 1500.50,
  "best_selling_medication": "Acetaminophen",
  "start_date": "2023-12-25",
  "end_date": "2024-01-01"
}
```

 
- **Status Code:**  `200` OK


#### 3. Monthly Sales Report 
- **URL:**  [http://127.0.0.1:8000/api/reports/monthly-sales]() 
- **Method:**  GET 
- **Authorization:**  Required (Bearer Token and Warehouse Owner role) 
- **Parameters:** 
- None 
- **Request Example:** 
- No additional parameters required. 
- **Response Example (Success):** 

```json
{
  "report_type": "Monthly Sales Report",
  "number_of_orders": 45,
  "number_of_pharmacists": 15,
  "quantities_sold": 320,
  "total_prices": 4000.25,
  "best_selling_medication": "Acetaminophen",
  "start_date": "2023-12-01",
  "end_date": "2023-12-31"
}
```

 
- **Status Code:**  `200` OK
