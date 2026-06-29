# Huong Dan Chay Va Demo A5 Analytics - Buoi 6

File nay dung cho demo rieng nhom A5. Cach chay nay khong sua cach chay full product trong `docker-compose.yml`; A5 dung file rieng `docker-compose.analytics.yml` de tranh anh huong cac nhom khac.

## 1. Tinh trang hien tai tren may

Repo da nam o:

```powershell
C:\Users\linzi\Downloads\lab4\DV_KetNoi_A
```

Nhanh dang dung:

```text
group-a5-analytics
```

Container A5 dang chay that tren Docker:

```powershell
docker compose -f docker-compose.analytics.yml ps
```

Ky vong thay:

```text
smart-campus-analytics   Up ... (healthy)   0.0.0.0:8000->8000/tcp
```

A5 dang nhan data that tu HiveMQ. Neu muon xem nhanh:

```powershell
curl.exe -s http://localhost:8000/health
```

Ket qua tot:

```json
{
  "status": "ok",
  "service": "analytics",
  "mqtt_connected": true,
  "total_events": 537
}
```

`total_events` se tiep tuc tang neu broker HiveMQ co data tu cac nhom/bo simulator.

## 2. Neu can chay lai tu dau

Mo PowerShell tai repo:

```powershell
cd C:\Users\linzi\Downloads\lab4\DV_KetNoi_A
```

Neu chua co `.env`, tao tu file mau:

```powershell
Copy-Item .env.example .env -Force
```

Build va chay rieng A5:

```powershell
docker compose -f docker-compose.analytics.yml up -d --build analytics
```

Kiem tra container:

```powershell
docker compose -f docker-compose.analytics.yml ps
```

Kiem tra health:

```powershell
curl.exe -s http://localhost:8000/health
```

Mo API docs:

```text
http://localhost:8000/docs
```

Kiem tra log dang nhan MQTT:

```powershell
docker compose -f docker-compose.analytics.yml logs --tail=80 analytics
```

Can thay cac dong:

```text
Connected to HiveMQ
Subscribed: smart-campus/events/sensor
Subscribed: smart-campus/events/access
Subscribed: smart-campus/events/camera
Subscribed: smart-campus/events/core-alert
Received smart-campus/events/sensor
Received smart-campus/events/access
```

## 3. Lenh xem data cho thay

### 3.1. Health

```powershell
curl.exe -s http://localhost:8000/health
```

Noi voi thay:

```text
Day la endpoint bat buoc /health. Service dang song, MQTT connected = true,
va total_events la so event A5 da nhan tu cac topic Product A.
```

### 3.2. Metrics dashboard

```powershell
curl.exe -s -H "Authorization: Bearer smart-campus-dev-token-2026" http://localhost:8000/api/v1/metrics
```

Noi voi thay:

```text
Day la output chinh cua A5. Service tong hop KPI tu cac event MQTT:
- sensor_events: so event cam bien tu A1 IoT
- access_events: so event quet the tu A3 Gate
- camera_events: so event camera tu A2 Camera neu A2 publish
- core_alerts: so alert tu A6 Core neu Core publish
- avg_temperature_by_room: nhiet do trung binh theo phong
- avg_humidity_by_room: do am trung binh theo phong
- access_deny_rate_percent: ti le tu choi ra vao
```

### 3.3. Recent events

```powershell
curl.exe -s -H "Authorization: Bearer smart-campus-dev-token-2026" "http://localhost:8000/api/v1/events/recent?limit=10"
```

Noi voi thay:

```text
Day la minh chung A5 nhan payload that tu MQTT. Moi item co topic,
payload event, va thoi diem A5 nhan duoc.
```

### 3.4. Log xu ly

```powershell
docker compose -f docker-compose.analytics.yml logs --tail=120 analytics
```

Noi voi thay:

```text
Log cho thay A5 subscribe 4 topic va xu ly tung message.
Moi lan nhan event, service in ra topic va tong so message da nhan.
```

### 3.5. API docs

Mo trinh duyet:

```text
http://localhost:8000/docs
```

Neu chay tren may khac/Radmin:

```text
http://<IP_MAY_A5>:8000/docs
```

API docs hien cac endpoint:

```text
GET /health
GET /api/v1/metrics
GET /api/v1/events/recent
```

Token mac dinh:

```text
smart-campus-dev-token-2026
```

## 4. Luu minh chung truoc khi demo

Chay cac lenh nay de refresh file trong `reports/`:

```powershell
docker compose -f docker-compose.analytics.yml ps | Set-Content -Encoding UTF8 reports\analytics-docker-compose-ps.txt
curl.exe -s http://localhost:8000/health | Set-Content -Encoding UTF8 reports\analytics-health-local.json
curl.exe -s -H "Authorization: Bearer smart-campus-dev-token-2026" http://localhost:8000/api/v1/metrics | Set-Content -Encoding UTF8 reports\analytics-metrics.json
curl.exe -s -H "Authorization: Bearer smart-campus-dev-token-2026" "http://localhost:8000/api/v1/events/recent?limit=10" | Set-Content -Encoding UTF8 reports\analytics-recent-events.json
docker compose -f docker-compose.analytics.yml logs --tail=200 analytics | Set-Content -Encoding UTF8 reports\analytics-live-logs.txt
```

Cac file can cho thay xem:

```text
reports/analytics-docker-compose-ps.txt
reports/analytics-health-local.json
reports/analytics-metrics.json
reports/analytics-recent-events.json
reports/analytics-live-logs.txt
reports/analytics-readiness-checklist.md
```

## 5. Test de nhom khac goi A5

Neu dung cung hotspot/Radmin, nhom khac khong goi `localhost`.
Nhom khac goi IP may demo A5:

```powershell
curl.exe http://<IP_MAY_A5>:8000/health
```

Lay IP tren Windows:

```powershell
ipconfig
```

Neu dung Radmin VPN, lay Radmin IP trong app Radmin va noi voi doi tac:

```text
http://<RADMIN_IP_A5>:8000/health
```

## 6. Script trinh bay 1 phut

Co the noi theo thu tu nay:

```text
Nhom em la A5 Analytics trong Product A.
Vai tro cua A5 la consumer MQTT va provider REST.

Input cua A5 la 4 topic:
1. A1 IoT publish sensor event vao smart-campus/events/sensor.
2. A3 Access Gate publish access event vao smart-campus/events/access.
3. A2 Camera se publish camera event vao smart-campus/events/camera.
4. A6 Core publish alert vao smart-campus/events/core-alert.

A5 subscribe cac topic do, validate doc du lieu JSON, dem so event,
tinh nhiet do/do am trung binh theo phong, tinh ti le access denied,
va luu recent events de truy vet.

Output cua A5 la REST API:
GET /health de kiem tra service song va MQTT connected.
GET /api/v1/metrics de tra dashboard KPI.
GET /api/v1/events/recent de xem payload MQTT vua nhan.

Sau day em demo:
docker compose ps de thay container running healthy.
GET /health de thay MQTT connected true.
GET /api/v1/metrics de thay so lieu tong hop.
GET /api/v1/events/recent va docker logs de thay payload/log xu ly that.
```

## 7. Diem can noi ro de tranh bi bat loi

```text
A5 hien da nhan du lieu that tu topic sensor va access.
A2 Camera can xac nhan publish topic smart-campus/events/camera thi camera_events moi tang.
Core alert cung se tang khi A6 publish smart-campus/events/core-alert.

Khi goi qua may khac, phai dung IP may A5/Radmin IP, khong dung Docker service name
va khong dung localhost cua may khac.
```

## 8. Dung service sau demo

Chi dung khi da demo xong:

```powershell
docker compose -f docker-compose.analytics.yml down
```

Neu dang chuan bi cham, khong chay lenh `down`; cu de container chay de `total_events` tiep tuc tang.
