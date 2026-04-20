# Code Examples

## Python

### Installation

```bash
pip install requests
```

### Basic Usage

```python
import requests
import json

TOKEN = "suk_f8fV8YTM76Ly08tixxpfZAsrwzIUms3xzdtCBHlcgvNNsHZtStVDPbgwnXamj0feC4YJt7y3Y5wIXfNya5rHzU1ON-X6mL38Sv25kc4IizMeKyH3m8_KAghp077GZu-W"
PROJECT_ID = "be02a6b2-0061-4849-983b-26504bde1144"
BASE_URL = "http://localhost:8000"

headers = {
    "Authorization": f"Bearer {TOKEN}",
    "Content-Type": "application/json"
}

# Get transcripts
response = requests.get(
    f"{BASE_URL}/api/transcripts/{PROJECT_ID}/",
    headers=headers
)
print(response.json())
```

### Login

```python
import requests

login_data = {
    "username": "sophyadmin",
    "password": "password123"
}

response = requests.post(
    "http://localhost:8000/api/auth/login/",
    json=login_data
)

if response.status_code == 200:
    token = response.json()["token"]
    print(f"Token: {token}")
else:
    print(f"Login failed: {response.json()}")
```

### Create New Token

```python
import requests

TOKEN = "suk_..."
headers = {
    "Authorization": f"Bearer {TOKEN}",
    "Content-Type": "application/json"
}

token_data = {
    "type": "service",
    "name": "Batch Processing",
    "expires_at": "2026-12-31T23:59:59Z"
}

response = requests.post(
    "http://localhost:8000/api/auth/tokens/",
    json=token_data,
    headers=headers
)

new_token = response.json()
print(f"Created token: {new_token['token']}")
```

### Fetch Transcripts with Filtering

```python
import requests
from datetime import datetime, timedelta

TOKEN = "suk_..."
PROJECT_ID = "be02a6b2-0061-4849-983b-26504bde1144"

headers = {"Authorization": f"Bearer {TOKEN}"}

# Get WhatsApp transcripts
params = {"platform": "whatsapp"}
response = requests.get(
    f"http://localhost:8000/api/transcripts/{PROJECT_ID}/",
    headers=headers,
    params=params
)

transcripts = response.json()
for transcript in transcripts:
    print(f"{transcript['user']['name']} - {transcript['platform']}")
```

### Batch Processing

```python
import requests
import time

TOKEN = "suk_..."
PROJECT_ID = "be02a6b2-0061-4849-983b-26504bde1144"

headers = {"Authorization": f"Bearer {TOKEN}"}

def fetch_all_dialogues():
    """Fetch all dialogues from a project"""
    response = requests.get(
        f"http://localhost:8000/api/transcripts/{PROJECT_ID}/dialogues/",
        headers=headers
    )
    
    if response.status_code == 200:
        return response.json()
    else:
        print(f"Error: {response.status_code}")
        return None

# Process dialogues
dialogues = fetch_all_dialogues()
if dialogues:
    for dialogue in dialogues:
        if dialogue["role"] == "user":
            print(f"User message: {dialogue['message']}")
        elif dialogue["role"] == "assistant":
            print(f"AI response: {dialogue['message']}")
```

---

## JavaScript / Node.js

### Installation

```bash
npm install axios
```

### Basic Usage

```javascript
const axios = require('axios');

const TOKEN = "suk_...";
const PROJECT_ID = "be02a6b2-0061-4849-983b-26504bde1144";
const BASE_URL = "http://localhost:8000";

const headers = {
  "Authorization": `Bearer ${TOKEN}`,
  "Content-Type": "application/json"
};

// Get transcripts
axios.get(`${BASE_URL}/api/transcripts/${PROJECT_ID}/`, { headers })
  .then(res => console.log(res.data))
  .catch(err => console.error(err.response.data));
```

### Login

```javascript
const axios = require('axios');

const loginData = {
  username: "sophyadmin",
  password: "password123"
};

axios.post("http://localhost:8000/api/auth/login/", loginData)
  .then(res => {
    const token = res.data.token;
    console.log(`Token: ${token}`);
  })
  .catch(err => console.error(`Login failed: ${err.response.data}`));
```

### Fetch with Filters

```javascript
const axios = require('axios');

const TOKEN = "suk_...";
const PROJECT_ID = "be02a6b2-0061-4849-983b-26504bde1144";

const headers = { "Authorization": `Bearer ${TOKEN}` };

// Get completed dialogues
axios.get(`http://localhost:8000/api/transcripts/${PROJECT_ID}/dialogues/`, {
  headers,
  params: { status: "completed", role: "user" }
})
  .then(res => {
    res.data.forEach(dialogue => {
      console.log(`${dialogue.role}: ${dialogue.message}`);
    });
  })
  .catch(err => console.error(err.response.data));
```

### Class-based Client

```javascript
class SophyAPIClient {
  constructor(token, baseUrl = "http://localhost:8000") {
    this.token = token;
    this.baseUrl = baseUrl;
    this.headers = {
      "Authorization": `Bearer ${token}`,
      "Content-Type": "application/json"
    };
  }

  async getTranscripts(projectId, filters = {}) {
    try {
      const response = await axios.get(
        `${this.baseUrl}/api/transcripts/${projectId}/`,
        { headers: this.headers, params: filters }
      );
      return response.data;
    } catch (error) {
      console.error("Error fetching transcripts:", error.response.data);
      throw error;
    }
  }

  async getDialogues(projectId, filters = {}) {
    try {
      const response = await axios.get(
        `${this.baseUrl}/api/transcripts/${projectId}/dialogues/`,
        { headers: this.headers, params: filters }
      );
      return response.data;
    } catch (error) {
      console.error("Error fetching dialogues:", error.response.data);
      throw error;
    }
  }

  async createToken(type, name, expiresAt = null) {
    try {
      const response = await axios.post(
        `${this.baseUrl}/api/auth/tokens/`,
        { type, name, expires_at: expiresAt },
        { headers: this.headers }
      );
      return response.data;
    } catch (error) {
      console.error("Error creating token:", error.response.data);
      throw error;
    }
  }
}

// Usage
const client = new SophyAPIClient("suk_...");
client.getTranscripts("project-id", { platform: "whatsapp" })
  .then(transcripts => console.log(transcripts));
```

---

## cURL

### Login

```bash
curl -X POST http://localhost:8000/api/auth/login/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "sophyadmin",
    "password": "password123"
  }' | jq .token
```

### List Tokens

```bash
TOKEN="suk_..."

curl -X GET http://localhost:8000/api/auth/tokens/ \
  -H "Authorization: Bearer ${TOKEN}" | jq .
```

### Create Token

```bash
TOKEN="suk_..."

curl -X POST http://localhost:8000/api/auth/tokens/ \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "service",
    "name": "Batch Job",
    "expires_at": "2026-12-31T23:59:59Z"
  }' | jq .token
```

### Get Transcripts

```bash
TOKEN="suk_..."
PROJECT_ID="be02a6b2-0061-4849-983b-26504bde1144"

curl -X GET "http://localhost:8000/api/transcripts/${PROJECT_ID}/?platform=whatsapp" \
  -H "Authorization: Bearer ${TOKEN}" | jq .
```

### Get Dialogues

```bash
TOKEN="suk_..."
PROJECT_ID="be02a6b2-0061-4849-983b-26504bde1144"

curl -X GET "http://localhost:8000/api/transcripts/${PROJECT_ID}/dialogues/?status=completed" \
  -H "Authorization: Bearer ${TOKEN}" | jq .
```

### Using jq for JSON parsing

```bash
# Pretty print
curl ... | jq .

# Select specific fields
curl ... | jq '.[].message'

# Filter
curl ... | jq '.[] | select(.status == "completed")'

# Map
curl ... | jq 'map({role: .role, message: .message})'
```

---

## Postman

1. **Create Collection** → "Sophy API"
2. **Add Environment Variable:**
   - `token`: suk_...
   - `base_url`: http://localhost:8000
   - `project_id`: be02a6b2-0061-4849-983b-26504bde1144

3. **Request Examples:**

**Login:**
```
POST {{base_url}}/api/auth/login/
Body (JSON):
{
  "username": "sophyadmin",
  "password": "password123"
}
```

**List Transcripts:**
```
GET {{base_url}}/api/transcripts/{{project_id}}/
Headers:
- Authorization: Bearer {{token}}
```

**Create Token:**
```
POST {{base_url}}/api/auth/tokens/
Headers:
- Authorization: Bearer {{token}}
Body (JSON):
{
  "type": "service",
  "name": "My Service Token"
}
```
