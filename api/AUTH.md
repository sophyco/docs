# Authentication API

## Overview

User authentication ve API token management.

---

## 1. Login (Get Initial Token)

Kullanıcı giriş yap ve default token al.

```http
POST /api/auth/login/
Content-Type: application/json

{
  "username": "your_username",
  "password": "your_password"
}
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| username | string | ✅ | Username |
| password | string | ✅ | Password |

### Response (200 OK)

```json
{
  "token": "suk_f8fV8YTM76Ly08tixxpfZAsrwzIUms3xzdtCBHlcgvNNsHZtStVDPbgwnXamj0feC4YJt7y3Y5wIXfNya5rHzU1ON-X6mL38Sv25kc4IizMeKyH3m8_KAghp077GZu-W",
  "user": {
    "id": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
    "username": "your_username",
    "email": "user@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "role": "agent",
    "organization_id": "8c9b5c94-1c9e-4b5f-8f5c-5c5c5c5c5c5c"
  }
}
```

### Example

```bash
curl -X POST http://localhost:8000/api/auth/login/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "sophyadmin",
    "password": "password123"
  }'
```

---

## 2. List User Tokens

Authenticated kullanıcının tüm tokenlarını listele.

```http
GET /api/auth/tokens/
Authorization: Bearer suk_...
```

### Response (200 OK)

```json
[
  {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "token": "suk_f8fV8YTM76Ly08tixxpfZAsrwzIUms3xzdtCBHlcgvNNsHZtStVDPbgwnXamj0feC4YJt7y3Y5wIXfNya5rHzU1ON-X6mL38Sv25kc4IizMeKyH3m8_KAghp077GZu-W",
    "type": "user",
    "name": "Default Token",
    "user_id": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
    "is_active": true,
    "expires_at": null,
    "last_used_at": "2026-04-20T10:30:00Z",
    "is_expired": false,
    "is_valid": true,
    "created_at": "2026-04-20T09:00:00Z",
    "updated_at": "2026-04-20T10:30:00Z"
  },
  {
    "id": "550e8400-e29b-41d4-a716-446655440001",
    "token": "ssk_aB1cD2eF3gH4iJ5kL6mN7oPq8rSt9uVwXyZ0aBcDeFgHiJkLmNoPqRsT...",
    "type": "service",
    "name": "Batch Processing",
    "user_id": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
    "is_active": true,
    "expires_at": "2026-12-31T23:59:59Z",
    "last_used_at": "2026-04-20T10:00:00Z",
    "is_expired": false,
    "is_valid": true,
    "created_at": "2026-04-15T14:20:00Z",
    "updated_at": "2026-04-20T10:00:00Z"
  }
]
```

### Example

```bash
curl -X GET http://localhost:8000/api/auth/tokens/ \
  -H "Authorization: Bearer suk_..."
```

---

## 3. Create New Token

Yeni API token oluştur.

```http
POST /api/auth/tokens/
Authorization: Bearer suk_...
Content-Type: application/json

{
  "type": "user|service",
  "name": "Production API Key",
  "expires_at": "2026-12-31T23:59:59Z"
}
```

### Request Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| type | string | ✅ | `user` veya `service` |
| name | string | ✅ | Token adı/açıklaması (max 255 chars) |
| expires_at | ISO 8601 | ❌ | Token expiry date. Boş bırakılırsa unlimited. |

### Response (201 Created)

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440002",
  "token": "suk_newtoken7gH4iJ5kL6mN7oPq8rSt9uVwXyZ0aBcDeFgHiJkLmNoPqRsT...",
  "type": "user",
  "name": "Production API Key",
  "user_id": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
  "is_active": true,
  "expires_at": "2026-12-31T23:59:59Z",
  "last_used_at": null,
  "is_expired": false,
  "is_valid": true,
  "created_at": "2026-04-20T10:35:00Z",
  "updated_at": "2026-04-20T10:35:00Z"
}
```

### Example

```bash
curl -X POST http://localhost:8000/api/auth/tokens/ \
  -H "Authorization: Bearer suk_..." \
  -H "Content-Type: application/json" \
  -d '{
    "type": "service",
    "name": "Batch Processing Job",
    "expires_at": "2026-12-31T23:59:59Z"
  }'
```

---

## 4. Update Token

Token'ı güncelle (is_active, expires_at).

```http
PATCH /api/auth/tokens/{id}/
Authorization: Bearer suk_...
Content-Type: application/json

{
  "is_active": false,
  "expires_at": "2026-06-30T23:59:59Z"
}
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| id | UUID | Token ID |

### Request Body (Optional)

| Field | Type | Description |
|-------|------|-------------|
| is_active | boolean | Token'ı aktif/pasif et |
| expires_at | ISO 8601 | Yeni expiry date |
| name | string | Token adını değiştir |

### Response (200 OK)

```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "token": "suk_...",
  "type": "user",
  "name": "Updated Token Name",
  "user_id": "f47ac10b-58cc-4372-a567-0e02b2c3d479",
  "is_active": false,
  "expires_at": "2026-06-30T23:59:59Z",
  "last_used_at": "2026-04-20T10:30:00Z",
  "is_expired": false,
  "is_valid": false,
  "created_at": "2026-04-20T09:00:00Z",
  "updated_at": "2026-04-20T10:40:00Z"
}
```

### Example

```bash
# Token'ı deaktif et
curl -X PATCH http://localhost:8000/api/auth/tokens/550e8400-e29b-41d4-a716-446655440000/ \
  -H "Authorization: Bearer suk_..." \
  -H "Content-Type: application/json" \
  -d '{
    "is_active": false
  }'

# Expiry date değiştir
curl -X PATCH http://localhost:8000/api/auth/tokens/550e8400-e29b-41d4-a716-446655440000/ \
  -H "Authorization: Bearer suk_..." \
  -H "Content-Type: application/json" \
  -d '{
    "expires_at": "2026-06-30T23:59:59Z"
  }'
```

---

## 5. Delete Token

Token'ı sil.

```http
DELETE /api/auth/tokens/{id}/
Authorization: Bearer suk_...
```

### Path Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| id | UUID | Token ID |

### Response (204 No Content)

Boş response, başarılı deletion.

### Example

```bash
curl -X DELETE http://localhost:8000/api/auth/tokens/550e8400-e29b-41d4-a716-446655440000/ \
  -H "Authorization: Bearer suk_..."
```

---

## Token Fields Explanation

| Field | Type | Description |
|-------|------|-------------|
| id | UUID | Token unique identifier |
| token | string | Actual token value (`suk_*` or `ssk_*`) |
| type | string | `user` or `service` |
| name | string | Human-readable token name |
| user_id | UUID | Token owner's user ID |
| is_active | boolean | Token deactivation flag |
| expires_at | ISO 8601 | Expiration date (null = never) |
| last_used_at | ISO 8601 | Timestamp of last API call |
| is_expired | boolean | Computed field: whether token expired |
| is_valid | boolean | Computed field: is_active && !is_expired |
| created_at | ISO 8601 | Token creation timestamp |
| updated_at | ISO 8601 | Token last update timestamp |

---

## Authentication Header Format

```http
Authorization: Bearer {token}
```

**Example:**
```http
Authorization: Bearer suk_f8fV8YTM76Ly08tixxpfZAsrwzIUms3xzdtCBHlcgvNNsHZtStVDPbgwnXamj0feC4YJt7y3Y5wIXfNya5rHzU1ON-X6mL38Sv25kc4IizMeKyH3m8_KAghp077GZu-W
```

---

## Admin Panel

Admin panelden tokenları yönetebilirsin:

```
URL: /sophy-admin/organizations/apitoken/
```

**Features:**
- Token oluştur/sil
- Expiry date belirle
- is_active toggle
- Token'ı kopyala (formatted display)
