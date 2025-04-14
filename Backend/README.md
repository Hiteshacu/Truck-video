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

---

# API Documentation - Captain Routes

## Endpoint: `/captains/register`

### Description
This endpoint is used to register a new captain in the system. It validates the input data, checks if the captain already exists, hashes the password, and creates a new captain in the database. Upon successful registration, it returns a JSON Web Token (JWT) and the captain details.

### Method
`POST`

### Request Body
```json
{
  "fullname": {
    "firstname": "string", // required, min length: 3
    "lastname": "string" // optional, min length: 3
  },
  "email": "string", // required, valid email format
  "password": "string", // required, min length: 6
  "vehicle": {
    "color": "string", // required, min length: 3
    "plate": "string", // required, min length: 3
    "capacity": 1, // required, integer, min: 1
    "vehicleType": "string" // required, one of: 'flattruck', 'boxtruck', 'truck'
  }
}
```

### Responses

#### Success
- **Status Code:** `201 Created`
- **Response Body:**
  ```json
  {
    "token": "string", // JWT token
    "captain": {
      "_id": "string", // unique ID of the captain
      "fullname": {
        "firstname": "string",
        "lastname": "string"
      },
      "email": "string",
      "vehicle": {
        "color": "string",
        "plate": "string",
        "capacity": 1,
        "vehicleType": "string"
      }
    }
  }
  ```

#### Errors
- **Status Code:** `400 Bad Request`
  - **Reason:** Validation errors or captain already exists.
  - **Response Body:**
    ```json
    {
      "errors": [
        {
          "msg": "string", // error message
          "param": "string", // field name
          "location": "string" // body, query, or params
        }
      ]
    }
    ```
    OR
    ```json
    {
      "message": "Captain already exist"
    }
    ```

---

## Endpoint: `/captains/login`

### Description
This endpoint is used to authenticate an existing captain. It validates the input data, checks if the captain exists, and verifies the password. Upon successful authentication, it returns a JSON Web Token (JWT) and the captain details.

### Method
`POST`

### Request Body
```json
{
  "email": "string", // required, valid email format
  "password": "string" // required, min length: 6
}
```

### Responses

#### Success
- **Status Code:** `200 OK`
- **Response Body:**
  ```json
  {
    "token": "string", // JWT token
    "captain": {
      "_id": "string", // unique ID of the captain
      "fullname": {
        "firstname": "string",
        "lastname": "string"
      },
      "email": "string",
      "vehicle": {
        "color": "string",
        "plate": "string",
        "capacity": 1,
        "vehicleType": "string"
      }
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
          "msg": "string", // error message
          "param": "string", // field name
          "location": "string" // body, query, or params
        }
      ]
    }
    ```

- **Status Code:** `401 Unauthorized`
  - **Reason:** Invalid email or password.
  - **Response Body:**
    ```json
    {
      "message": "Invalid email or password"
    }
    ```

---

## Endpoint: `/captains/profile`

### Description
This endpoint is used to retrieve the profile of the currently authenticated captain.

### Method
`GET`

### Headers
```json
{
  "Authorization": "Bearer <JWT token>" // required
}
```

### Responses

#### Success
- **Status Code:** `200 OK`
- **Response Body:**
  ```json
  {
    "_id": "string", // unique ID of the captain
    "fullname": {
      "firstname": "string",
      "lastname": "string"
    },
    "email": "string",
    "vehicle": {
      "color": "string",
      "plate": "string",
      "capacity": 1,
      "vehicleType": "string"
    }
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

## Endpoint: `/captains/logout`

### Description
This endpoint is used to log out the currently authenticated captain. It clears the authentication token and blacklists it to prevent reuse.

### Method
`GET`

### Headers
```json
{
  "Authorization": "Bearer <JWT token>" // required
}
```

### Responses

#### Success
- **Status Code:** `200 OK`
- **Response Body:**
  ```json
  {
    "message": "Logout successfully"
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