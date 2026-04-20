# Best Practices

## Security

### 1. Token Management

✅ **DO:**
- Store tokens in environment variables
- Rotate tokens quarterly
- Use service keys (`ssk_`) for automation
- Set expiry dates on production tokens
- Revoke unused tokens

❌ **DON'T:**
- Hardcode tokens in source code
- Commit tokens to version control
- Share tokens via email/chat
- Use same token across projects
- Create unlimited tokens without monitoring

**Example (Secure):**
```python
import os
from dotenv import load_dotenv

load_dotenv()
TOKEN = os.getenv('SOPHY_API_TOKEN')  # From .env file
```

### 2. Authentication Headers

Always use Bearer token:
```http
Authorization: Bearer suk_...
```

Never expose token in URL:
```
❌ /api/transcripts?token=suk_...
✅ Header: Authorization: Bearer suk_...
```

### 3. Token Rotation

**Schedule:**
- User tokens: every 6 months
- Service tokens: every 3 months
- Compromised tokens: immediately

**Process:**
```python
# Create new token
new_token = create_token("service", "Production Rotation")

# Update all services
update_services_with_new_token(new_token)

# Monitor old token usage for 24h
wait(24 hours)

# Revoke old token
revoke_token(old_token_id)
```

---

## Performance

### 1. Filtering

Filter on API side when possible:
```bash
# ✅ Good: Filter on API
curl "http://localhost:8000/api/transcripts/{projectId}/?platform=whatsapp"

# ❌ Bad: Fetch all, filter locally
curl "http://localhost:8000/api/transcripts/{projectId}/" | \
  jq '.[] | select(.platform == "whatsapp")'
```

### 2. Batch Operations

```python
# ❌ Don't: Make multiple requests
for project_id in project_ids:
    response = requests.get(f".../{project_id}/")
    process(response.json())

# ✅ Do: Batch requests with asyncio
import asyncio
import aiohttp

async def fetch_all():
    async with aiohttp.ClientSession() as session:
        tasks = [
            session.get(f".../{pid}/", headers=headers)
            for pid in project_ids
        ]
        return await asyncio.gather(*tasks)
```

### 3. Caching

```python
from functools import lru_cache
from datetime import datetime, timedelta

class CachedSophyClient:
    def __init__(self, token):
        self.token = token
        self.cache = {}
        self.cache_ttl = timedelta(minutes=5)
    
    def get_transcripts(self, project_id):
        cache_key = f"transcripts_{project_id}"
        
        if cache_key in self.cache:
            cached_at, data = self.cache[cache_key]
            if datetime.now() - cached_at < self.cache_ttl:
                return data
        
        # Fetch from API
        response = requests.get(
            f"http://localhost:8000/api/transcripts/{project_id}/",
            headers={"Authorization": f"Bearer {self.token}"}
        )
        data = response.json()
        
        # Cache it
        self.cache[cache_key] = (datetime.now(), data)
        return data
```

### 4. Pagination (Future)

When pagination is implemented:
```bash
# Get first page
curl "...?page=1&page_size=50"

# Get next page
curl "...?page=2&page_size=50"
```

---

## Error Handling

### 1. Retry Logic

```python
import time
import requests

def retry_request(url, headers, max_retries=3, backoff=2):
    for attempt in range(max_retries):
        try:
            response = requests.get(url, headers=headers, timeout=10)
            if response.status_code < 500:  # Don't retry client errors
                return response
        except requests.RequestException as e:
            if attempt < max_retries - 1:
                wait_time = backoff ** attempt
                time.sleep(wait_time)
            else:
                raise
    
    return None
```

### 2. Error Logging

```python
import logging

logger = logging.getLogger(__name__)

def api_call(endpoint):
    try:
        response = requests.get(endpoint)
        response.raise_for_status()
        return response.json()
    except requests.exceptions.HTTPError as e:
        logger.error(f"HTTP Error {e.response.status_code}: {e.response.text}")
        raise
    except requests.exceptions.RequestException as e:
        logger.error(f"Request error: {e}")
        raise
```

### 3. Rate Limiting (Future)

When rate limiting is implemented:
```python
from time import sleep

def rate_limited_request(url, headers, requests_per_second=10):
    delay = 1.0 / requests_per_second
    sleep(delay)
    return requests.get(url, headers=headers)
```

---

## Monitoring & Alerting

### 1. Token Usage Monitoring

```python
def check_token_health(token_id):
    """Monitor token usage and expiry"""
    token = get_token(token_id)
    
    # Alert if expiring soon
    days_until_expiry = (token.expires_at - datetime.now()).days
    if days_until_expiry < 7:
        send_alert(f"Token {token.name} expires in {days_until_expiry} days")
    
    # Alert if not used recently
    if token.last_used_at:
        days_since_use = (datetime.now() - token.last_used_at).days
        if days_since_use > 30:
            send_alert(f"Token {token.name} not used for {days_since_use} days")
```

### 2. Authentication Failures

```python
# Track failed auth attempts
failed_attempts = {}

def track_failed_auth(token):
    failed_attempts[token] = failed_attempts.get(token, 0) + 1
    
    if failed_attempts[token] > 5:
        # Alert: potential security issue
        send_alert(f"Token {token} has {failed_attempts[token]} failed attempts")
        # Optionally revoke token
        revoke_token(token)
```

### 3. Performance Metrics

```python
import time

def measure_performance(endpoint):
    start = time.time()
    response = requests.get(endpoint)
    duration = time.time() - start
    
    # Log performance
    logger.info(f"{endpoint} took {duration:.2f}s")
    
    # Alert if slow
    if duration > 5:
        send_alert(f"Slow request: {endpoint} took {duration:.2f}s")
    
    return response
```

---

## Integration Patterns

### 1. Service-to-Service

```python
# Service A calls Service B through Sophy API
class SophyServiceClient:
    def __init__(self, service_key):
        self.service_key = service_key
    
    def get_conversation_data(self, conversation_id):
        return requests.get(
            f"http://localhost:8000/api/transcripts/{conversation_id}/",
            headers={"Authorization": f"Bearer {self.service_key}"}
        ).json()
```

### 2. Scheduled Jobs

```python
from apscheduler.schedulers.background import BackgroundScheduler

def fetch_and_process():
    transcripts = get_transcripts(PROJECT_ID)
    for transcript in transcripts:
        process_transcript(transcript)

scheduler = BackgroundScheduler()
scheduler.add_job(fetch_and_process, 'interval', hours=1)
scheduler.start()
```

### 3. Webhooks (Future)

When webhook support is added:
```python
# Receive webhook events
@app.route('/webhook/transcript-updated', methods=['POST'])
def handle_transcript_update():
    data = request.json
    signature = request.headers.get('X-Sophy-Signature')
    
    # Verify webhook signature
    if not verify_signature(data, signature):
        return "Unauthorized", 401
    
    # Process event
    process_transcript_update(data)
    return "OK", 200
```

---

## Testing

### 1. Unit Tests

```python
import unittest
from unittest.mock import patch, MagicMock

class TestSophyAPI(unittest.TestCase):
    @patch('requests.get')
    def test_get_transcripts(self, mock_get):
        # Mock response
        mock_get.return_value.json.return_value = [
            {"_id": "1", "message": "Hello"}
        ]
        
        # Test
        result = get_transcripts("project-id")
        
        # Assert
        self.assertEqual(len(result), 1)
        self.assertEqual(result[0]["message"], "Hello")
```

### 2. Integration Tests

```python
class TestSophyIntegration(unittest.TestCase):
    def setUp(self):
        self.token = os.getenv('TEST_TOKEN')
        self.project_id = os.getenv('TEST_PROJECT_ID')
    
    def test_auth_flow(self):
        # Login
        token = login("user", "pass")
        self.assertIsNotNone(token)
        
        # Fetch data
        transcripts = get_transcripts(self.project_id, token)
        self.assertIsInstance(transcripts, list)
```

### 3. Load Testing

```bash
# Using Apache Bench
ab -n 1000 -c 10 -H "Authorization: Bearer suk_..." \
  http://localhost:8000/api/transcripts/PROJECT_ID/

# Using wrk
wrk -t12 -c400 -d30s \
  -H "Authorization: Bearer suk_..." \
  http://localhost:8000/api/transcripts/PROJECT_ID/
```

---

## Compliance

### 1. Data Privacy

- Don't log full token values
- Mask sensitive data in logs
- Implement data retention policies
- Support GDPR (right to be forgotten)

### 2. Audit Trail

```python
def log_api_access(token_user, endpoint, method):
    AuditLog.objects.create(
        user=token_user,
        endpoint=endpoint,
        method=method,
        timestamp=datetime.now(),
        ip_address=request.remote_addr
    )
```

### 3. Rate Limiting (Future)

Implement rate limiting per token:
```
- User tokens: 1000 requests/hour
- Service tokens: 10000 requests/hour
- Burst: 100 requests/minute
```
