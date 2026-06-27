# A5 Analytics Readiness Checklist - Buoi 6

## Role

- [x] A5 Analytics is an MQTT consumer for Product A events.
- [x] A5 Analytics is a REST provider for dashboard metrics.
- [x] A5 exposes `GET /health` on host port `8000`.

## Inputs

- [x] Subscribes `smart-campus/events/sensor` from A1 IoT Ingestion.
- [x] Subscribes `smart-campus/events/access` from A3 Access Gate.
- [ ] A2 Camera must confirm/publish `smart-campus/events/camera`.
- [x] Subscribes `smart-campus/events/core-alert` from A6 Core Business.

## Processing

- [x] Counts sensor/access/camera/core-alert events.
- [x] Computes average temperature and humidity by room.
- [x] Computes access deny rate.
- [x] Stores recent MQTT events for demo inspection.
- [x] Logs each received MQTT topic.

## Outputs

- [x] `GET /health` returns `200 OK`.
- [x] `GET /api/v1/metrics` returns dashboard counters.
- [x] `GET /api/v1/metrics/{metric_name}` returns one metric.
- [x] `GET /api/v1/events/recent` returns recent MQTT payloads.

## Evidence Generated Locally

- [x] `reports/analytics-docker-compose-ps.txt`
- [x] `reports/analytics-health-local.json`
- [x] `reports/analytics-metrics.json`
- [x] `reports/analytics-recent-events.json`
- [x] `reports/analytics-live-logs.txt`

## Demo Notes

- Use `docker compose -f docker-compose.analytics.yml ps` for A5 standalone demo.
- Ask partner groups to call `http://<A5_DEMO_IP>:8000/health`.
- If using Radmin VPN, publish the A5 Radmin IP and keep port `8000`.
- Do not claim A2 camera integration is complete until A2 publishes `smart-campus/events/camera` with the agreed schema.

