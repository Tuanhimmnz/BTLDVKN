# Huong Dan Clone, Build Va Chay Docker Tren May Khac

Tai lieu nay danh cho nguoi clone repo ve may moi de build/chay project bang Docker.

## 1. Yeu Cau Tren May Moi

Can cai san:

- Git
- Docker Desktop hoac Docker Engine
- Docker Compose V2

Kiem tra:

```powershell
git --version
docker --version
docker compose version
```

Tren Windows, nen mo Docker Desktop truoc khi chay lenh `docker compose`.

## 2. Clone Source Code

```powershell
git clone https://github.com/Tuanhimmnz/BTLDVKN.git
cd BTLDVKN
```

Code moi cua nhom A5 nam tren branch `group-a5-analytics`.
Sau khi clone, checkout branch nay:

```powershell
git fetch origin
git checkout group-a5-analytics
```

Neu muon xem tat ca branch:

```powershell
git branch -a
```

## 3. Tao File Cau Hinh `.env`

File `.env` khong nen commit len Git. Tren may moi, tao tu file mau:

```powershell
Copy-Item .env.example .env
```

Neu dung macOS/Linux:

```bash
cp .env.example .env
```

Mo `.env` va kiem tra cac gia tri quan trong:

```text
MQTT_BROKER_HOST
MQTT_BROKER_PORT
MQTT_IOT_USERNAME
MQTT_IOT_PASSWORD
MQTT_GATE_USERNAME
MQTT_GATE_PASSWORD
AUTH_TOKEN
ANALYTICS_PUBLIC_PORT
```

Neu port `8000` dang bi ung dung khac dung, doi:

```text
ANALYTICS_PUBLIC_PORT=8008
```

Sau do goi API bang `http://localhost:8008`.

## 4. Cach Chay Rieng A5 Analytics

Cach nay phu hop voi repo hien tai vi project dang co service A5 Analytics va file:

```text
docker-compose.analytics.yml
services/analytics/Dockerfile
```

Build image va chay container:

```powershell
docker compose -f docker-compose.analytics.yml up -d --build
```

Kiem tra container:

```powershell
docker compose -f docker-compose.analytics.yml ps
```

Kiem tra health:

```powershell
curl.exe http://localhost:8000/health
```

Neu da doi `ANALYTICS_PUBLIC_PORT`, thay `8000` bang port moi.

Mo dashboard HTML tren trinh duyet:

```text
http://localhost:8000/
```

API docs:

```text
http://localhost:8000/docs
```

Kiem tra nhanh endpoint health:

```text
http://localhost:8000/health
```

Token mac dinh de goi endpoint metrics/recent events:

```text
smart-campus-dev-token-2026
```

Neu ban doi `AUTH_TOKEN` trong `.env`, thay token trong lenh curl/Postman bang token moi.

Xem log:

```powershell
docker compose -f docker-compose.analytics.yml logs -f analytics
```

Dung service:

```powershell
docker compose -f docker-compose.analytics.yml down
```

## 5. Test API A5

Health khong can token:

```powershell
curl.exe http://localhost:8000/health
```

Lay metrics can bearer token:

```powershell
curl.exe -H "Authorization: Bearer smart-campus-dev-token-2026" http://localhost:8000/api/v1/metrics
```

Xem event MQTT gan nhat:

```powershell
curl.exe -H "Authorization: Bearer smart-campus-dev-token-2026" "http://localhost:8000/api/v1/events/recent?limit=10"
```

Neu doi `AUTH_TOKEN` trong `.env`, thay token trong lenh curl bang token moi.

Mo dashboard bang trinh duyet:

```text
http://localhost:8000/
```

## 6. Cach Build Lai Khi Co Thay Doi Code

Neu sua code Python hoac Dockerfile:

```powershell
docker compose -f docker-compose.analytics.yml up -d --build
```

Neu muon build lai sach hon:

```powershell
docker compose -f docker-compose.analytics.yml down
docker compose -f docker-compose.analytics.yml build --no-cache
docker compose -f docker-compose.analytics.yml up -d
```

Xoa image/container khong dung nua:

```powershell
docker system prune
```

Chi chay lenh prune khi da chac khong can cac container/image cu.

## 7. Cach Chay Full Product Neu May Khac Co Du A1-A7

File `docker-compose.yml` khai bao 7 service:

```text
iot-ingestion
camera-stream
access-gate
ai-vision
analytics
core-business
notification
```

Lenh chay full:

```powershell
docker compose up -d --build
```

Kiem tra:

```powershell
docker compose ps
```

Health cac service:

```powershell
curl.exe http://localhost:8001/health
curl.exe http://localhost:8002/health
curl.exe http://localhost:8003/health
curl.exe http://localhost:8004/health
curl.exe http://localhost:8005/health
curl.exe http://localhost:8006/health
curl.exe http://localhost:8007/health
```

Luu y: lenh full product chi build duoc neu repo/may do co du cac thu muc:

```text
services/iot_ingestion
services/camera_stream
services/access_gate
services/ai_vision
services/analytics
services/core_business
services/notification
```

Neu thieu cac thu muc tren, hay chay rieng A5 bang `docker-compose.analytics.yml`.

## 8. Cho May Khac Goi API A5

Neu chay tren may A va may B muon goi API, may B khong dung `localhost`.
May B phai dung IP cua may A:

```text
http://<IP_MAY_CHAY_DOCKER>:8000/health
```

Lay IP tren Windows:

```powershell
ipconfig
```

Neu dung Radmin VPN, lay Radmin IP cua may dang chay Docker.

Vi du:

```powershell
curl.exe http://26.x.x.x:8000/health
```

Neu Windows Firewall chan port, mo inbound rule cho port `8000` hoac port da cau hinh trong `ANALYTICS_PUBLIC_PORT`.

## 9. Loi Thuong Gap

### Docker chua chay

Loi thuong gap:

```text
Cannot connect to the Docker daemon
```

Cach xu ly: mo Docker Desktop va cho den khi Docker san sang.

### Port da bi dung

Loi thuong gap:

```text
Bind for 0.0.0.0:8000 failed: port is already allocated
```

Cach xu ly: doi `ANALYTICS_PUBLIC_PORT` trong `.env`, vi du:

```text
ANALYTICS_PUBLIC_PORT=8008
```

Sau do chay lai:

```powershell
docker compose -f docker-compose.analytics.yml up -d --build
```

### Health ok nhung MQTT chua connected

Kiem tra log:

```powershell
docker compose -f docker-compose.analytics.yml logs --tail=100 analytics
```

Kiem tra lai trong `.env`:

```text
MQTT_BROKER_HOST
MQTT_BROKER_PORT
MQTT_IOT_USERNAME
MQTT_IOT_PASSWORD
MQTT_GATE_USERNAME
MQTT_GATE_PASSWORD
```

### API metrics tra ve 401

Endpoint `/api/v1/metrics` can header:

```text
Authorization: Bearer <AUTH_TOKEN>
```

Mac dinh trong file mau:

```text
AUTH_TOKEN=smart-campus-dev-token-2026
```

## 10. Lenh Nhanh Tom Tat

```powershell
git clone https://github.com/Tuanhimmnz/BTLDVKN.git
cd BTLDVKN
git checkout group-a5-analytics
Copy-Item .env.example .env
docker compose -f docker-compose.analytics.yml up -d --build
docker compose -f docker-compose.analytics.yml ps
curl.exe http://localhost:8000/health
curl.exe -H "Authorization: Bearer smart-campus-dev-token-2026" http://localhost:8000/api/v1/metrics
```

Mo dashboard:

```text
http://localhost:8000/
```
