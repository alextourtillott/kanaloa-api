# Kanaloa Interactive API Reference

Welcome to the Kanaloa Developer ecosystem! Whether you're writing automation scripts for Autonomous Underwater Vehicle (AUV) telemetry deployment, or syncing real-time marine telemetry to an institutional database, our REST API provides an adaptable integration layer built to scale.

Our API features a live sandbox below. Plug in your credentials, adjust the payload parameters, and test your requests in real-time.

---

## Base URL

All production API requests must be made over HTTPS to the following base endpoint:
`[https://api.kanaloa-ocean.io/v1](https://api.kanaloa-ocean.io/v1)`

---

## Authentication

Kanaloa uses Bearer Token authentication. Pass your secret API key in the `Authorization` header of all requests.

```http
Authorization: Bearer YOUR_API_KEY
```

> **Security Note:** Keep your API keys hidden. Never commit raw keys to public repositories or expose them in client-side front-end code.

---

## Endpoint: Create AUV Mission Sync

### `POST /missions/sync`

This endpoint allows Marine Robotic Engineers to register a new automated reef seeding or monitoring flight path, linking live telemetry checkpoints directly to site ID ecosystems.

### Request Body Parameters

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `site_id` | `string` | **Yes** | The unique identifier for the targeted ocean restoration zone. |
| `auv_id` | `string` | **Yes** | The hardware tracking ID of the deployed marine drone. |
| `operation_type` | `enum` | **Yes** | The specific purpose of the dive. Allowed: `["seeding", "telemetry_scan", "sample_collection"]`. |
| `coordinates` | `array` | **Yes** | An array of geo-coordinates `[latitude, longitude, depth_meters]` mapping the AUV flight path grid. |

---

## Live Code & Interactive Sandbox

Select your preferred language environment to generate a code snippet, or use the interactive fields to query our live test sandbox immediately.

### Step 1: Configure Payload

```json
{
  "site_id": "reef-zone-7",
  "auv_id": "triton-explorer-04",
  "operation_type": "telemetry_scan",
  "coordinates": [
    [-14.2705, -170.6722, 12.5],
    [-14.2710, -170.6725, 14.2]
  ]
}

```

### Step 2: Generate Code Snippet

#### Python Requests

```python
import requests

url = "https://api.kanaloa-ocean.io/v1/missions/sync"
headers = {
    "Authorization": "Bearer YOUR_API_KEY",
    "Content-Type": "application/json"
}
payload = {
    "site_id": "reef-zone-7",
    "auv_id": "triton-explorer-04",
    "operation_type": "telemetry_scan",
    "coordinates": [[-14.2705, -170.6722, 12.5], [-14.2710, -170.6725, 14.2]]
}

response = requests.post(url, json=payload, headers=headers)
print(response.status_code)
print(response.json())

```

#### JavaScript Fetch

```javascript
const payload = {
  site_id: "reef-zone-7",
  auv_id: "triton-explorer-04",
  operation_type: "telemetry_scan",
  coordinates: [[-14.2705, -170.6722, 12.5], [-14.2710, -170.6725, 14.2]]
};

fetch('https://api.kanaloa-ocean.io/v1/missions/sync', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer YOUR_API_KEY',
    'Content-Type': 'application/json'
  },
  body: JSON.stringify(payload)
})
.then(res => res.json())
.then(data => console.log(data));

```

#### cURL

```bash
curl -X POST https://api.kanaloa-ocean.io/v1/missions/sync \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "site_id": "reef-zone-7",
    "auv_id": "triton-explorer-04",
    "operation_type": "telemetry_scan",
    "coordinates": [[-14.2705, -170.6722, 12.5], [-14.2710, -170.6725, 14.2]]
  }'

```

---

### Step 3: Try the Sandbox Execution

Clicking the button simulates hitting our mock-sandbox environment using your customized parameters above.

---

## Expected Server Responses

### `201 Created`

Returns when the telemetry coordinates match structural parameters and the mission is successfully registered to the site dataset.

```json
{
  "mission_id": "msn_88310_alpha",
  "status": "queued",
  "sync_timestamp": "2026-07-17T23:51:00Z",
  "adaptive_routing": {
    "optimized_depth_override_applied": false,
    "recommended_velocity_knots": 2.4
  }
}

```

### `400 Bad Request`

Returns if missing required schema values, or coordinate formatting fails validation constraints (e.g. depth exceeding valid bounds).

```json
{
  "error": "bad_request",
  "message": "Validation failed: coordinates field must contain numeric values mapping [lat, long, depth]."
}

```