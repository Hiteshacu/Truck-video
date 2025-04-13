# API Documentation

## Endpoint: `/users/register`

### Description
This endpoint is used to register a new user in the system. It validates the input data, checks if the user already exists, hashes the password, and creates a new user in the database. Upon successful registration, it returns a JSON Web Token (JWT) and the user details.

### Method
`POST`

### Request Body
The following fields are required in the request body:

```json
{
  "fullname": {
    "firstname": "string (min length: 3, required)",
    "lastname": "string (min length: 3, optional)"
  },
  "email": "string (valid email format, required)",
  "password": "string (min length: 6, required)"
}
```

### Validation Rules
- `fullname.firstname`: Must be at least 3 characters long.
- `fullname.lastname`: Optional, but if provided, must be at least 3 characters long.
- `email`: Must be a valid email address.
- `password`: Must be at least 6 characters long.

### Responses

#### Success
- **Status Code:** `201 Created`
- **Response Body:**
  ```json
  {
    "token": "string (JWT token)",
    "user": {
      "_id": "string",
      "fullname": {
        "firstname": "string",
        "lastname": "string"
      },
      "email": "string"
    }
  }
  ```

#### Errors
- **Status Code:** `400 Bad Request`
  - **Reason:** Validation errors or user already exists.
  - **Response Body:**
    ```json
    {
      "errors": [
        {
          "msg": "string (error message)",
          "param": "string (field name)",
          "location": "string (body)"
        }
      ]
    }
    ```
    OR
    ```json
    {
      "message": "User already exist"
    }
    ```

---

## Endpoint: `/users/login`

### Description
This endpoint is used to authenticate an existing user. It validates the input data, checks if the user exists, and verifies the password. Upon successful authentication, it returns a JSON Web Token (JWT) and the user details.

### Method
`POST`

### Request Body
The following fields are required in the request body:

```json
{
  "email": "string (valid email format, required)",
  "password": "string (min length: 6, required)"
}
```

### Validation Rules
- `email`: Must be a valid email address.
- `password`: Must be at least 6 characters long.

### Responses

#### Success
- **Status Code:** `200 OK`
- **Response Body:**
  ```json
  {
    "token": "string (JWT token)",
    "user": {
      "_id": "string",
      "fullname": {
        "firstname": "string",
        "lastname": "string"
      },
      "email": "string"
    }
  }
  ```

#### Errors
- **Status Code:** `400 Bad Request`
  - **Reason:** Validation errors.
  - **Response Body:**
    ```json
    {
      "errors": [
        {
          "msg": "string (error message)",
          "param": "string (field name)",
          "location": "string (body)"
        }
      ]
    }
    ```

- **Status Code:** `401 Unauthorized`
  - **Reason:** Invalid email or password.
  - **Response Body:**
    ```json
    {
      "message": "Invalid user or password"
    }
    ```

### Example Request
```bash
POST /users/login HTTP/1.1
Content-Type: application/json

{
  "email": "john.doe@example.com",
  "password": "password123"
}
```

### Example Response
#### Success
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "_id": "643d2f9b8f1b2c001c8e4e5d",
    "fullname": {
      "firstname": "John",
      "lastname": "Doe"
    },
    "email": "john.doe@example.com"
  }
}
```

#### Error
```json
{
  "message": "Invalid user or password"
}
```