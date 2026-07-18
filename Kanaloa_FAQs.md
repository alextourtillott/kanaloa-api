# Kanaloa: Frequently Asked Questions

Welcome to the Kanaloa FAQs. Here you'll find answers to the most common questions regarding platform integration, data models, hardware communication, and edge computing capabilities for our application users.

---

### 1: What is Kanaloa, and how does it integrate with existing marine robotics software?

Kanaloa is a cloud-native, REST and WebSockets-driven API designed to bridge the gap between oceanographic data ingestion and real-time robotic orchestration. For software developers, Kanaloa integrates directly with the **(ROS) Robot Operating System** and **MOOS-IvP** ecosystems.

You can feed raw sensor telemetry, e.g. side-scan sonar, CTD profiles, or hydrophone arrays into Kanaloa's endpoints. The API processes this data through our machine learning models and returns optimized pathfinding coordinates or behavioral states back to the vehicle's control stack, via low-latency WebSocket streams.

---

### 2: How does the API handle low-bandwidth, high-latency underwater acoustic telemetry?

Ocean environments present severe communication constraints. Kanaloa addresses this by utilizing a highly compressed, binary serialization format based on **Protocol Buffers (Protobuf)**.

Our synchronization protocol features:

* **Delta Compression:** The API only transmits changes in telemetry or state values rather than the entire data payload.
* **Intermittent Connection Resiliency:** The client-side SDK includes a local SQLite caching mechanism. If an Autonomous Underwater Vehicle (AUV) loses its acoustic uplink, data is queued locally and automatically flushed chronologically once a link is re-established.
* **Adaptive Bitrate Quality:** Kanaloa dynamically drops optional environmental layers, e.g., high-resolution localized temperature gradients to prioritize critical navigation and control packets when bandwidth drops below 1 kbps.

---

### 3: Can I run Kanaloa models directly on my deployment vessel or AUV hardware?

Yes. While the primary API runs on a scalable cloud infrastructure, Kanaloa offers **Kanaloa Edge**, a containerized runtime designed to deploy directly onto field hardware.

Kanaloa Edge is distributed as a lightweight Docker image optimized for ARM64 architectures, like NVIDIA Jetson Orin or Raspberry Pi Compute Module 4. It allows marine robotics engineers to execute localized habitat classification, bio-acoustic threat detection, and real-time obstacle avoidance directly on the edge without requiring an active satellite or acoustic uplink to the cloud.

---

### 4: What data standards does Kanaloa support for marine biology and habitat mapping?

To ensure interoperability across the global scientific community, Kanaloa strictly enforces established oceanographic and spatial data standards:

* **Spatial Data:** All geographic payloads use the `EPSG:4326` (WGS 84) coordinate reference system, formatted in GeoJSON or Cloud Optimized GeoTIFFs (COG).
* **Biological Observation:** Marine biology tracking endpoints map directly to the **Darwin Core (DwC)** standard, making it seamless to export data directly to repositories like OBIS (Ocean Biodiversity Information System).
* **Environmental Sensor Data:** Time-series telemetry aligns with the **NetCDF4** data format, utilizing Climate and Forecast (CF) metadata conventions for standardized naming attributes.

---

### 5: How do I authenticate and manage rate limits for fleet deployment?

Kanaloa uses **OAuth 2.0 Client Credentials Grant** for machine-to-machine authentication, issuing short-lived (JWTs) JSON Web Tokens to your surface vessels or edge gateways.

Rate limits are calculated on a **per-fleet** rather than per-device basis to accommodate unexpected surface bursts when an AUV uploads a large block of cached data.

* **Developer Tier:** 600 requests/minute burst, 10,000 total requests/day.
* **Enterprise Field Tier:** 3,000 requests/minute burst, with unlimited asynchronous batch uploads via dedicated bulk ingestion endpoints.

If an edge device exceeds a rate limit during a high-frequency telemetry burst, Kanaloa responds with an HTTP `429 Too Many Requests` status code along with a `Retry-After` header. Our SDKs handle this automatically using an exponential backoff algorithm with jitter.

---