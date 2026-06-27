# Analytics MQTT Event Contract

Analytics is an MQTT consumer and REST provider.

## Inputs

| Producer | Topic | Purpose | Required fields used by Analytics |
|---|---|---|---|
| A1 IoT Ingestion | `smart-campus/events/sensor` | Environmental KPI aggregation | `status`, `location`, `temperature_c`, `humidity_percent`, `battery_percent` |
| A3 Access Gate | `smart-campus/events/access` | Access KPI aggregation | `access_result` |
| A2 Camera Stream | `smart-campus/events/camera` | Camera event count | any valid JSON object |
| A6 Core Business | `smart-campus/events/core-alert` | Alert count | any valid JSON object |

Analytics accepts both snake_case and camelCase variants for common fields such as
`temperature_c`/`temperatureC`, `humidity_percent`/`humidityPercent`,
`battery_percent`/`batteryPercent`, and `access_result`/`accessResult`.

## Outputs

| Method | Path | Purpose |
|---|---|---|
| `GET` | `/health` | Runtime and MQTT status |
| `GET` | `/api/v1/metrics` | Aggregated dashboard counters |
| `GET` | `/api/v1/metrics/{metric_name}` | Single metric lookup |
| `GET` | `/api/v1/events/recent?limit=20` | Recent MQTT events received by Analytics |

Protected REST endpoints require:

```text
Authorization: Bearer <AUTH_TOKEN>
```

## Error handling

- Invalid JSON payloads are logged and do not stop the service.
- Missing optional fields are handled with defaults.
- MQTT disconnect updates `/health.mqtt_connected` to `false`; Paho reconnects when the broker is available.
