# [PROJECT_NAME] — API Contract Documentation

This document is the **source of truth** for the JSON request/response shapes that the backend must implement and the frontend must consume.

---

## Index

0. [Conventions](#conventions)
1. [Authentication](#1-authentication)
2. [User & Profile](#2-user--profile)
3. [[Domain Area 1]](#3-domain-area-1)
4. [[Domain Area 2]](#4-domain-area-2)
N. [[...]]

---

## Conventions

### Success response

```json
{
  "success": true,
  "data": { },
  "message": "Operation completed successfully"
}
```

### Error response

```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable description"
  }
}
```

### Required Headers

**Authenticated:**
```http
Authorization: Bearer <id_token>
Content-Type: application/json
```

**Public:**
```http
Content-Type: application/json
```

### Status Codes

| Code | Usage |
|------|-------|
| 200 | OK |
| 201 | Created |
| 400 | Validation error |
| 401 | Unauthenticated |
| 403 | Forbidden / subscription required |
| 404 | Not Found |
| 409 | Conflict |
| 429 | Rate limited |
| 500 | Internal server error |

### Numeric values

All numbers are sent and received as JSON `number`. The backend converts to the storage type (`Decimal` for DynamoDB, etc.) before persistence.

---

## 1. Authentication

### POST /auth/register

Creates a new user (and tenant, if applicable).

**Request**
```json
{
  "email": "user@example.com",
  "password": "Strong#Pass1",
  "name": "Jane Doe",
  "[organization_name]": "Acme Inc"
}
```

**Response 201**
```json
{
  "success": true,
  "data": {
    "user_id": "uuid",
    "[organization_id]": "uuid",
    "requires_confirmation": true
  },
  "message": "User registered. Confirm your email."
}
```

**Errors**
- `400 VALIDATION_ERROR`
- `409 EMAIL_ALREADY_EXISTS`

### POST /auth/login

**Request**
```json
{ "email": "user@example.com", "password": "Strong#Pass1" }
```

**Response 200**
```json
{
  "success": true,
  "data": {
    "id_token": "ey...",
    "access_token": "ey...",
    "refresh_token": "ey...",
    "expires_in": 3600
  }
}
```

**Errors**
- `401 INVALID_CREDENTIALS`
- `403 USER_NOT_CONFIRMED`

> Repeat this pattern for every endpoint. Group by domain area. Always include: HTTP method + path, request body, response body, error codes, and (optionally) curl example.

---

## 2. User & Profile

### GET /user/profile

**Response 200**
```json
{
  "success": true,
  "data": {
    "user_id": "uuid",
    "email": "user@example.com",
    "name": "Jane Doe",
    "[organization_id]": "uuid",
    "role": "admin"
  }
}
```

---

## 3. [Domain Area 1]

### GET /[resource]

**Query parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `cursor` | string | no | Pagination cursor |
| `limit` | int | no | Page size (default 20, max 100) |

**Response 200**
```json
{
  "success": true,
  "data": {
    "items": [],
    "next_cursor": null
  }
}
```

### POST /[resource]

**Request**
```json
{ "name": "Example" }
```

**Response 201**
```json
{
  "success": true,
  "data": { "id": "uuid", "name": "Example", "created_at": "2026-01-01T00:00:00Z" }
}
```

---

## Curl Examples

```bash
# Register
curl -X POST "$BASE_URL/auth/register" \
  -H "Content-Type: application/json" \
  -d '{"email":"u@e.com","password":"Strong#Pass1","name":"Jane","organization_name":"Acme"}'

# Authenticated GET
curl "$BASE_URL/user/profile" -H "Authorization: Bearer $TOKEN"
```

---

**Last Updated**: [YYYY-MM]
