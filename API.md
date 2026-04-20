# Sophy Communication Platform - API Documentation

## Overview

RESTful API için authentication, token management, ve transcript/dialogue retrieval. Tüm endpoints Bearer Token authentication kullanır.

**Base URL:** `http://localhost:8000`

## API Modules

- **[Authentication](api/AUTH.md)** - Login, token creation & management
- **[Transcripts & Dialogues](api/TRANSCRIPTS.md)** - Data retrieval & filtering
- **[Error Handling](api/ERRORS.md)** - Error codes & responses
- **[Code Examples](api/EXAMPLES.md)** - Python, JavaScript, cURL örnekleri
- **[Best Practices](api/BEST_PRACTICES.md)** - Security & performance

## Token Types

```
User Key (suk_)     → Personal API token
Service Key (ssk_)  → Application/service account token
```

Example:
```
suk_f8fV8YTM76Ly08tixxpfZAsrwzIUms3xzdtCBHlcgvNNsHZtStVDPbgwnXamj0feC4YJt7y3Y5wIXfNya5rHzU1ON-X6mL38Sv25kc4IizMeKyH3m8_KAghp077GZu-W
ssk_aB1cD2eF3gH4iJ5kL6mN7oPq8rSt9uVwXyZ0aBcDeFgHiJkLmNoPqRsT...
```

## Quick Start

```bash
# 1. Login
curl -X POST http://localhost:8000/api/auth/login/ \
  -d '{"username":"user","password":"pass"}' | jq .token

# 2. Get Transcripts
curl -X GET http://localhost:8000/api/transcripts/{projectId}/ \
  -H "Authorization: Bearer {token}"
```

---

**Version:** 1.0.0  
**Last Updated:** 2026-04-20
