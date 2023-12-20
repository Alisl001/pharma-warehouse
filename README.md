# Pharma-Warehouse API Documentation

## Introduction

This document outlines the functionalities and usage of the Pharma-Warehouse APIs. It serves as a reference for front-end developers integrating with the backend system.

### 1. Register 
- **URL:**  `http://127.0.0.1:8000/api/register` 
- **Method:**  `POST` 
- **Parameters:**  
- `name` (required): User's name 
- `phone` (required, unique): User's phone number 
- `password` (required, confirmed): User's password 
- **Request Example:** 

```json
{
  "name": "ALi",
  "phone": "0947580141",
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
    "name": "Ali",
    "phone": "0947580141",
    "created_at": "2023-01-01T12:00:00Z",
    "updated_at": "2023-01-01T12:00:00Z"
  }
}
``` 
- **Response Example (Error):** 

```json
{
  "error": "Validation failed"
}
```

### 2. Login 
- **URL:**  `http://127.0.0.1:8000/api/login` 
- **Method:**  `POST` 
- **Parameters:**  
- `phone` (required): User's phone number 
- `password` (required): User's password 
- **Request Example:** 

```json
{
  "phone": "0947580141",
  "password": "password123"
}
``` 
- **Response Example (Success):** 

```json
{
  "access_token": "your_access_token",
  "token_type": "bearer",
  "expires_in": 3600,
  "user": {
    // User details
  }
}
``` 
- **Response Example (Error):** 

```json
{
  "error": "Unauthorized"
}
```
### 3. Logout 
- **URL:**  `http://127.0.0.1:8000/api/logout` 
- **Method:**  `POST` 
- **Authorization:**  Bearer Token 
- **Response Example:** 

```json
{
  "message": "User successfully signed out"
}
```
### 4. Add Medication 
- **URL:**  `http://127.0.0.1:8000/api/medication/add` 
- **Method:**  `POST` 
- **Authorization:**  Bearer Token 
- **Parameters:**  
- `scientific_name` (required): Medication's scientific name 
- `brand_name` (required): Medication's brand name 
- `classification` (required): Medication's classification 
- `manufacturer` (required): Medication's manufacturer 
- `available_quantity` (required): Available quantity 
- `expiry_date` (required): Expiry date 
- `price` (required): Medication's price 
- **Request Example:** 

```json
{
  "scientific_name": "Medicine A",
  "brand_name": "Brand A",
  "classification": "Class_A",
  "manufacturer": "Manufacturer A",
  "available_quantity": 100,
  "expiry_date": "2023-12-31",
  "price": 19.99
}
``` 
- **Response Example:** 

```json
{
  "message": "Medication added successfully"
}
```
### 5. Update Medication 
- **URL:**  `http://127.0.0.1:8000/api/medication/update/{id}` 
- **Method:**  `PUT` 
- **Authorization:**  Bearer Token 
- **Parameters:**  
- `id` (url parameter, required): Medication ID
- (Other parameters to update) 
- **Request Example:** 

```json
{
  "scientific_name": "Medicine A",
  "brand_name": "Brand A",
  "classification": "Class_A",
  "manufacturer": "Manufacturer A",
  "available_quantity": 100,
  "expiry_date": "2023-12-31",
  "price": 19.99
}
``` 
- **Response Example:** 

```json
{
  "message": "Medication updated successfully"
}
```
### 6. Delete Medication 
- **URL:**  `http://127.0.0.1:8000/api/medication/delete/{id}` 
- **Method:**  `DELETE` 
- **Authorization:**  Bearer Token 
- **Parameters:**  
- `id` (url parameter, required): Medication ID to delete 
- **Response Example:** 

```json
{
  "message": "Medication deleted successfully"
}
```
### 7. List All Medications 
- **URL:**  `http://127.0.0.1:8000/api/medications` 
- **Method:**  `GET` 
- **Authorization:**  Bearer Token 
- **Response Example:** 

```json
{
  "medications": [
    // List of medications
  ]
}
```
### 8. List Medications By Classification 
- **URL:**  `http://127.0.0.1:8000/api/medications/{classification}` 
- **Method:**  `GET` 
- **Authorization:**  Bearer Token 
- **Parameters:**  
- `classification` (url parameter, required): Medication classification 
- **Response Example:** 

```json
{
  "medications": [
    // List of medications for the specified classification
  ]
}
```

