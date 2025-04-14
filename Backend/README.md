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

---

## Endpoint: `/users/profile`

### Description
This endpoint is used to retrieve the profile of the currently authenticated user.

### Method
`GET`

### Headers
- `Authorization`: `Bearer <JWT token>` (required)

### Responses

#### Success
- **Status Code:** `200 OK`
- **Response Body:**
  ```json
  {
    "_id": "string",
    "fullname": {
      "firstname": "string",
      "lastname": "string"
    },
    "email": "string"
  }
  ```

#### Errors
- **Status Code:** `401 Unauthorized`
  - **Reason:** Missing or invalid token.
  - **Response Body:**
    ```json
    {
      "message": "Unauthorized"
    }
    ```

---

## Endpoint: `/users/logout`

### Description
This endpoint is used to log out the currently authenticated user. It clears the authentication token and blacklists it to prevent reuse.

### Method
`GET`

### Headers
- `Authorization`: `Bearer <JWT token>` (required)

### Responses

#### Success
- **Status Code:** `200 OK`
- **Response Body:**
  ```json
  {
    "message": "Logged out"
  }
  ```

#### Errors
- **Status Code:** `401 Unauthorized`
  - **Reason:** Missing or invalid token.
  - **Response Body:**
    ```json
    {
      "message": "Unauthorized"
    }
    ```