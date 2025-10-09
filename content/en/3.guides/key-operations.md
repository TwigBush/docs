---
title: Key Operations
description: Managing cryptographic keys for Resource Servers
navigation:
  icon: i-lucide-key-round
---

# Key Operations

TwigBush manages cryptographic keys for Resource Servers (RS) that need to authenticate with the Authorization Server (AS). Keys are indexed by their RFC 7638 JWK thumbprint to prevent collisions.

## Directory Structure

After initialization, your key directories look like this:

```
~/.twigbush/
├── config.yaml                     # Configuration file
├── keys/                           # Your RS private keys (client-side)
│   ├── key-{thumbprint}.jwk        # Private key
│   └── key-{thumbprint}.pub.jwk    # Public key
└── data/                           # AS data (server-side)
    └── rs_keys/                    # Registry of registered RS public keys
        ├── default/                # Default tenant
        │   └── {thumb256}.json     # Registered key record
        └── acme-corp/              # Custom tenant (multi-tenant)
            └── {thumb256}.json     # Registered key record
```

## Key Concepts

### Client Keys vs Registry
- **Client Keys** (`~/.twigbush/keys/`) - Your private/public key pairs used to authenticate as an RS
- **AS Registry** (`~/.twigbush/data/rs_keys/`) - Public keys registered with the AS from all RSs

### Thumbprints
Keys are identified by their **RFC 7638 JWK thumbprint**:
- **SHA-384** thumbprint for client keys (shown in CLI)
- **SHA-256** thumbprint for registry storage (used in GNAP)
- Collision-free identifier derived from the canonical public key

### Multi-Tenant Support
Keys can be registered to different tenants, allowing:
- Isolation between different organizations
- Same key registered to multiple tenants if needed
- Per-tenant key management

---

## CLI Commands

### Generate a New Key

Create a new ES384 key pair:
```bash
# Generate key only
twigbush keys new
# Generate and register immediately
twigbush keys new --register \
    --as http://localhost:8085 \
    --rs-id checkout-api
#Generate for specific tenant
twigbush keys new --register \
    --as http://localhost:8085 \
    --tenant acme-corp \
    --rs-id billing-api
``` 

**Output:**
```bash
Generating ES384 key... Wrote /Users/you/.twigbush/keys/key-K98LN...wAFj.jwk 
Thumbprint: K98LNvLj2UxxO2seI4YXL4FSB8YRx_YHybDorhieMNU2hHHZhPT5cFYp4Hi7wAFj 
✓ Key registered successfully for tenant 'default'
```

### Register an Existing Key

Register your public key with the AS:
```bash
# Register with default tenant
twigbush keys register \
    --as http://localhost:8085 \
    --rs-id checkout-api
# Register with specific tenant
twigbush keys register \
    --as http://localhost:8085 \
    --tenant acme-corp \
    --rs-id billing-api \
    --key ~/.twigbush/keys/key-*.jwk
# With admin authentication (not supported yet)
twigbush keys register \
    --as http://localhost:8085 \
    --tenant production \
    --rs-id payment-api \
    --admin-token "your-admin-token"
``` 

**Flags:**
- `--as` - Authorization Server base URL (required)
- `--tenant` - Tenant ID (default: "default")
- `--rs-id` - Resource Server identifier (required)
- `--key` - Path to private key file (defaults to config default_key)
- `--admin-token` - Admin bearer token for authentication (optional)
---

## REST API Endpoints

All RS key management endpoints support multi-tenant operations.

### Register a Key

**Endpoint:** `POST /admin/tenants/{tenant}/rs/keys`

Register a public key with the AS for a specific tenant.

**Request:**
```bash 
curl -X POST http://localhost:8085/admin/tenants/default/rs/keys
-H "Content-Type: application/json"
-d '{ "jwk": { "kty": "EC", "crv": "P-384", "x": "...", "y": "...", "kid": "key-id" }, "kid": "my-key-1", "alg": "ES384", "display_rs": "checkout-api" }'
```

**Using jq for proper JSON:**
```bash 
jq -n --argjson jwk "(cat ~/.twigbush/keys/*.pub.jwk)" \ '{ jwk:jwk, kid: ($jwk.kid // "auto"), alg: "ES384", display_rs: "checkout-api" }' | curl -X POST http://localhost:8085/admin/tenants/default/rs/keys
-H "Content-Type: application/json"
-d @-
``` 

**Response:**
```json 
{ "thumb256": "xXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxX", "kid": "my-key-1", "display_rs": "checkout-api" }
```

### List All Keys for a Tenant

**Endpoint:**
* `GET /admin/tenants/{tenant}/rs/keys`

**Request:**
```bash
# Default tenant
curl http://localhost:8085/admin/tenants/default/rs/keys
# Custom tenant
curl http://localhost:8085/admin/tenants/acme-corp/rs/keys
``` 

**Response:**
```json 
{ "keys":    }latex_unknown_tag
```

### Get Specific Key

**Endpoint:**
* `GET /admin/tenants/{tenant}/rs/keys/{thumb256}`

**Request:**
```bash 
curl http://localhost:8085/admin/tenants/default/rs/keys/xXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxX
``` 

**Response:**
```json 
{ "tenant": "default", "thumb256": "xXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxX", "kid": "my-key-1", "alg": "ES384", "pub_jwk": { "kty": "EC", "crv": "P-384", "x": "...", "y": "..." }, "active": true, "created_at": "2025-10-09T12:34:56Z", "display_rs": "checkout-api" }
```

### Deactivate/Rotate a Key

**Endpoint:** `DELETE /admin/tenants/{tenant}/rs/keys/{thumb256}`

Mark a key as inactive (soft delete for key rotation).

**Request:**
```bash 
curl -X DELETE http://localhost:8085/admin/tenants/default/rs/keys/xXxXxXxXxXxXxXxXxXxXxXxXxXxXxXxX
``` 

**Response:** HTTP 204 No Content

The key record is updated with `"active": false` and `"rotated_at"` timestamp.

---

## Multi-Tenant Examples

### Register Same Key to Multiple Tenants
```bash
# Register to default tenant
twigbush keys register \
        --tenant default \
        --rs-id shared-api 
# Register same key to another tenant
twigbush keys register \
        --tenant acme-corp \
        --rs-id shared-api
```

### REST API Multi-Tenant
```bash
Register to multiple tenants via API
for tenant in default acme-corp customer-123; do jq -n
--argjson jwk "(cat ~/.twigbush/keys/*.pub.jwk)" \ '{jwk:jwk, alg: "ES384", display_rs: "multi-tenant-api"}'
| curl -X POST "http://localhost:8085/admin/tenants/$tenant/rs/keys"
-H "Content-Type: application/json"
-d @- done
List keys per tenant
curl http://localhost:8085/admin/tenants/default/rs/keys | jq '.keys[].display_rs' curl http://localhost:8085/admin/tenants/acme-corp/rs/keys | jq '.keys[].display_rs'
``` 

---

## Key Rotation

When rotating keys:

1. **Generate new key:**
```bash
twigbush keys new
```

2. **Register new key:**
```bash
twigbush keys register --rs-id your-service
```

3. **Update your service to use new key**

4. **Deactivate old key:**
```bash
curl -X DELETE http://localhost:8085/admin/tenants/default/rs/keys/{old-thumb256}
```

The old key remains in the registry with `active: false` for audit purposes.

---

## Verification

After registering a key, verify it was stored:
```bash
# Check file system
ls -la ~/.twigbush/data/rs_keys/default/
# View stored record
cat ~/.twigbush/data/rs_keys/default/*.json | jq
# List via API
curl http://localhost:8085/admin/tenants/default/rs/keys | jq
``` 

