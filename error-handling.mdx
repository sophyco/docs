# Error Handling

## HTTP Status Codes

| Code | Name | Description |
|------|------|-------------|
| 200 | OK | Successful GET request |
| 201 | Created | Successful POST request |
| 204 | No Content | Successful DELETE request |
| 400 | Bad Request | Invalid request parameters |
| 401 | Unauthorized | Invalid/missing authentication |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 500 | Server Error | Internal server error |

---

## Error Response Format

```json
{
  "detail": "Error message here"
}
```

veya

```json
{
  "error": "Error message here"
}
```

---

## Common Errors

### 400 Bad Request

**Missing required parameter:**
```json
{
  "error": "project_id is required"
}
```

**Invalid token type:**
```json
{
  "type": ["\"invalid\" is not a valid choice. Expected one of: user, service"]
}
```

**Missing request body:**
```json
{
  "username": ["This field is required."],
  "password": ["This field is required."]
}
```

---

### 401 Unauthorized

**Invalid token:**
```json
{
  "detail": "Invalid token."
}
```

**Missing authorization header:**
```json
{
  "detail": "Authentication credentials were not provided."
}
```

**Expired token:**
```json
{
  "detail": "Token is inactive or expired."
}
```

**Inactive user:**
```json
{
  "detail": "User inactive."
}
```

---

### 403 Forbidden

**Cannot delete other user's token:**
```json
{
  "detail": "You can only delete your own tokens."
}
```

---

### 404 Not Found

**Token not found:**
```json
{
  "detail": "Not found."
}
```

**Project not found:**
```json
{
  "detail": "Not found."
}
```

---

### 500 Server Error

**Unexpected error:**
```json
{
  "detail": "Internal server error"
}
```

---

## Error Handling Best Practices

1. **Always check status code first**
   ```python
   if response.status_code != 200:
       print(f"Error: {response.json()}")
   ```

2. **Parse error message**
   ```python
   error_detail = response.json().get('detail') or response.json().get('error')
   ```

3. **Implement retry logic for 5xx errors**
   ```python
   import time
   max_retries = 3
   for attempt in range(max_retries):
       try:
           response = requests.get(url, headers=headers)
           if response.status_code < 500:
               break
       except requests.RequestException:
           time.sleep(2 ** attempt)
   ```

4. **Log authentication failures**
   - Track 401 errors
   - Monitor token expiry
   - Rotate expired tokens

---

## Debugging Tips

### Enable verbose logging

**Python:**
```python
import logging
logging.basicConfig(level=logging.DEBUG)
```

**cURL:**
```bash
curl -v -X GET http://localhost:8000/api/transcripts/{projectId}/ \
  -H "Authorization: Bearer {token}"
```

### Check token validity

```bash
curl -X GET http://localhost:8000/api/auth/tokens/ \
  -H "Authorization: Bearer {token}" | jq '.[] | select(.id == "token-id")'
```

### Verify authentication

```bash
# Test with invalid token
curl -X GET http://localhost:8000/api/transcripts/{projectId}/ \
  -H "Authorization: Bearer invalid_token"
# Should return 401
```
