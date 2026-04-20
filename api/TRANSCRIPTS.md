# Transcripts & Dialogues API

## Overview

Conversation transcripts ve dialogues'ı retrieve et ve filter'le.

**Authentication:** Tüm endpoints Bearer Token gereklidir (`suk_*` or `ssk_*`)

---

## 1. List Transcripts

Bir project'in transcriptlerini listele.

```http
GET /api/transcripts/{projectId}/
Authorization: Bearer suk_...
```

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| projectId | UUID | ✅ | Project ID |

### Query Parameters (Optional Filtering)

| Parameter | Type | Description |
|-----------|------|-------------|
| conversationID | string | Conversation ID'sine göre filtrele |
| sessionID | string | Session ID'sine göre filtrele |
| platform | string | Platform (whatsapp, webchat, vb.) |
| unread | boolean | Unread status (true/false) |

### Response (200 OK)

```json
[
  {
    "_id": "507f1f77bcf86cd799439011",
    "projectID": "be02a6b2-0061-4849-983b-26504bde1144",
    "conversationID": "6c92ff77-bcf8-6cd7-9943-9011507f1f77",
    "sessionID": "s123456789abcdef",
    "reportTags": ["urgent", "vip"],
    "unread": false,
    "platform": "whatsapp",
    "browser": "Chrome",
    "device": "desktop",
    "os": "macOS",
    "user": {
      "name": "John Doe",
      "image": "https://example.com/avatar.jpg"
    },
    "createdAt": "2026-04-20T09:00:00Z",
    "updatedAt": "2026-04-20T10:00:00Z"
  },
  {
    "_id": "507f1f77bcf86cd799439012",
    "projectID": "be02a6b2-0061-4849-983b-26504bde1144",
    "conversationID": "7d92ff77-bcf8-6cd7-9943-9011507f2f77",
    "sessionID": "s987654321fedcba",
    "reportTags": [],
    "unread": true,
    "platform": "webchat",
    "browser": "unknown",
    "device": "mobile",
    "os": "iOS",
    "user": {
      "name": "Jane Smith",
      "image": null
    },
    "createdAt": "2026-04-20T08:30:00Z",
    "updatedAt": "2026-04-20T08:45:00Z"
  }
]
```

### Examples

```bash
# Tüm transcriptleri listele
curl -X GET http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/ \
  -H "Authorization: Bearer suk_..."

# WhatsApp transcriptleri filtrele
curl -X GET "http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/?platform=whatsapp" \
  -H "Authorization: Bearer suk_..."

# Unread transcriptleri filtrele
curl -X GET "http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/?unread=true" \
  -H "Authorization: Bearer suk_..."

# Multiple filters kombinasyonu
curl -X GET "http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/?platform=whatsapp&unread=true&sessionID=s123456789abcdef" \
  -H "Authorization: Bearer suk_..."
```

---

## 2. List Dialogues

Bir project'in dialoguelerini listele.

```http
GET /api/transcripts/{projectId}/dialogues/
Authorization: Bearer suk_...
```

### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| projectId | UUID | ✅ | Project ID |

### Query Parameters (Optional Filtering)

| Parameter | Type | Description |
|-----------|------|-------------|
| conversationID | string | Conversation ID'sine göre filtrele |
| sessionID | string | Session ID'sine göre filtrele |
| role | string | Role (user, assistant, human, etc.) |
| format | string | Format (launch, request, trace, result) |
| type | string | Type (text, image, action, etc.) |
| status | string | Status (pending, processing, completed, cancelled) |

### Response (200 OK)

```json
[
  {
    "_id": "507f191e810c19729de860ea",
    "projectID": "be02a6b2-0061-4849-983b-26504bde1144",
    "conversationID": "6c92ff77-bcf8-6cd7-9943-9011507f1f77",
    "sessionID": "s123456789abcdef",
    "role": "user",
    "format": "request",
    "type": "text",
    "message": "Hello, how can you help me?",
    "input": {
      "role": "user",
      "content": "Hello, how can you help me?"
    },
    "payload": {
      "message_id": "msg-123",
      "platform": "whatsapp"
    },
    "parent_message_id": null,
    "proactive": false,
    "source": null,
    "status": "completed",
    "startTime": "2026-04-20T09:00:00Z"
  },
  {
    "_id": "507f191e810c19729de860eb",
    "projectID": "be02a6b2-0061-4849-983b-26504bde1144",
    "conversationID": "6c92ff77-bcf8-6cd7-9943-9011507f1f77",
    "sessionID": "s123456789abcdef",
    "role": "assistant",
    "format": "result",
    "type": "text",
    "message": "I'm here to assist you. What do you need?",
    "input": {
      "role": "assistant",
      "content": "I'm here to assist you. What do you need?"
    },
    "payload": {
      "message_id": "msg-124",
      "platform_raw": {}
    },
    "parent_message_id": "msg-123",
    "proactive": false,
    "source": "AI Agent",
    "status": "completed",
    "startTime": "2026-04-20T09:00:05Z"
  },
  {
    "_id": "507f191e810c19729de860ec",
    "projectID": "be02a6b2-0061-4849-983b-26504bde1144",
    "conversationID": "6c92ff77-bcf8-6cd7-9943-9011507f1f77",
    "sessionID": "s123456789abcdef",
    "role": "human",
    "format": "result",
    "type": "text",
    "message": "Thank you for your help!",
    "input": {
      "role": "assistant",
      "content": "Thank you for your help!",
      "from": "human"
    },
    "payload": {
      "message_id": "msg-125"
    },
    "parent_message_id": "msg-124",
    "proactive": false,
    "source": "Human Agent",
    "status": "completed",
    "startTime": "2026-04-20T09:00:30Z"
  }
]
```

### Examples

```bash
# Tüm dialoguları listele
curl -X GET http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/dialogues/ \
  -H "Authorization: Bearer suk_..."

# Completed dialoguları filtrele
curl -X GET "http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/dialogues/?status=completed" \
  -H "Authorization: Bearer suk_..."

# User mesajlarını filtrele
curl -X GET "http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/dialogues/?role=user" \
  -H "Authorization: Bearer suk_..."

# Assistant mesajlarını filtrele
curl -X GET "http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/dialogues/?role=assistant" \
  -H "Authorization: Bearer suk_..."

# Text type dialoguları filtrele
curl -X GET "http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/dialogues/?type=text" \
  -H "Authorization: Bearer suk_..."

# Multiple filters kombinasyonu
curl -X GET "http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/dialogues/?role=user&type=text&status=completed" \
  -H "Authorization: Bearer suk_..."

# Conversation'a göre filtrele
curl -X GET "http://localhost:8000/api/transcripts/be02a6b2-0061-4849-983b-26504bde1144/dialogues/?conversationID=6c92ff77-bcf8-6cd7-9943-9011507f1f77" \
  -H "Authorization: Bearer suk_..."
```

---

## Transcript Fields

| Field | Type | Description |
|-------|------|-------------|
| _id | ObjectId | MongoDB document ID |
| projectID | UUID | Project ID |
| conversationID | UUID | Conversation ID |
| sessionID | string | Session ID |
| reportTags | array | Custom tags (urgent, vip, etc.) |
| unread | boolean | Unread status |
| platform | string | Platform (whatsapp, webchat, vb.) |
| browser | string | Browser info |
| device | string | Device type (mobile, desktop) |
| os | string | Operating system |
| user | object | User info (name, image) |
| createdAt | ISO 8601 | Creation timestamp |
| updatedAt | ISO 8601 | Last update timestamp |

---

## Dialogue Fields

| Field | Type | Description |
|-------|------|-------------|
| _id | ObjectId | MongoDB document ID |
| projectID | UUID | Project ID |
| conversationID | UUID | Conversation ID |
| sessionID | string | Session ID |
| role | string | Message role (user, assistant, human, system, etc.) |
| format | string | Format (launch, request, trace, result) |
| type | string | Type (text, image, action, debug, etc.) |
| message | string | Message content |
| input | object | Input structure (role, content, etc.) |
| payload | object | Additional metadata |
| parent_message_id | string | Parent message reference |
| proactive | boolean | Proactive flag |
| source | string | Message source (AI Agent, Human Agent, etc.) |
| status | string | Status (pending, processing, completed, cancelled) |
| startTime | ISO 8601 | Message timestamp |

---

## Filter Value Examples

### role values
```
user        → Customer/user message
assistant   → AI assistant message
human       → Human agent message
system      → System message
developer   → Developer/tool message
```

### format values
```
launch  → Initial message
request → User request
trace   → Processing trace
result  → Final result
```

### type values
```
text    → Text message
image   → Image message
action  → Action trigger
debug   → Debug info
block   → Block/flow marker
```

### status values
```
pending     → Waiting to process
processing  → Currently processing
completed   → Successfully completed
cancelled   → Cancelled/failed
```

### platform values
```
whatsapp  → WhatsApp Business
webchat   → Web chat widget
telegram  → Telegram
slack     → Slack integration
```

---

## Pagination

Query params aracılığıyla pagination desteklenebilir (future feature).

```
?page=1&page_size=20
```

Şu an tüm results döndürülüyor.
