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
