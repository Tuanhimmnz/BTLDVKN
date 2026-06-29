# Review Bao Cao A5 Analytics

Ngay review: 2026-06-29

File Word da doc: `C:\Users\linzi\Downloads\NHOM_A5_Nguyen_Thanh_Trung.docx`

Repo review: `C:\Users\linzi\Downloads\lab4\DV_KetNoi_A`

## 1. Ket Qua Kiem Tra Nhanh

- Dashboard HTML da duoc khoi phuc lai tai `services/analytics/src/dashboard.html`.
- `main.py` dang phuc vu giao dien tai `GET /` va `GET /dashboard`.
- `main.py` compile thanh cong bang `python -m compileall services/analytics/src/main.py`.
- A5 standalone chay thanh cong bang `docker compose -f docker-compose.analytics.yml up -d --build analytics`.
- Container `smart-campus-analytics` dang `healthy`, port `8000 -> 8000`.
- MQTT connected thanh cong va da nhan event that tu HiveMQ.
- Full `docker-compose.yml` khong build duoc vi repo hien tai chi co `services/analytics`, thieu cac thu muc A1/A2/A3/A4/A6/A7.

## 2. Evidence Moi Da Tao

Text evidence:

- `reports/report-review/evidence/a5-docker-compose-ps.txt`
- `reports/report-review/evidence/a5-health.json`
- `reports/report-review/evidence/a5-metrics.json`
- `reports/report-review/evidence/a5-recent-events.json`
- `reports/report-review/evidence/a5-live-logs.txt`
- `reports/report-review/evidence/full-compose-build-result.txt`

Anh evidence moi:

- `reports/report-review/screenshots/01-a5-docker-compose-ps.png`
- `reports/report-review/screenshots/02-a5-health.png`
- `reports/report-review/screenshots/03-a5-metrics.png`
- `reports/report-review/screenshots/04-a5-recent-events.png`
- `reports/report-review/screenshots/05-a5-live-logs.png`
- `reports/report-review/screenshots/06-full-compose-build-failure.png`

Noi dung Word da duoc trich ra de doi chieu:

- `reports/report-review/word_extracted_text.txt`
- `reports/report-review/word_headings.txt`
- `reports/report-review/word_media_inventory.txt`
- `reports/report-review/word-media/`
- `reports/report-review/word-media-contact-sheet.png`

## 3. Loi Nghiem Trong Can Sua Truoc

### 3.1. Dashboard web da co lai, can bo sung anh minh chung

Trong Word co muc:

- Block `[221]`: `4.5. Dashboard giao dien web`
- Block `[222]`: noi A5 co giao dien dashboard web tai `GET /` va `GET /dashboard`, phuc vu bang `FileResponse`.
- Block `[223]` den `[229]`: mo ta card, chart, recent events, auto refresh.

Hien tai source da khoi phuc dashboard. Vi vay co the giu muc nay, nhung can bo sung evidence:

- Chup anh `http://localhost:8000/dashboard`.
- Chup anh `http://localhost:8000/docs` neu muon minh chung Swagger UI.
- Neu bao cao noi co bieu do, anh dashboard phai thay ro metric cards/bar chart/recent events.

### 3.2. Bao cao claim full 7 service/container nhung repo hien tai khong chay full duoc

Repo chi co:

```text
services/analytics
```

Lenh full compose:

```powershell
docker compose -f docker-compose.yml build
```

Ket qua that: fail vi thieu build context, vi du:

```text
unable to prepare context: path "...\\services\\camera_stream" not found
```

Can sua cac noi dung sau trong Word:

- Block `[212]`: neu noi `docker-compose.yml day du 7 service` thi phai ghi ro chi dung khi co source day du A1-A7.
- Block `[313]`: bang evidence dang ghi `Toan bo 7 container healthy`; hien tai khong chung minh duoc.
- Block `[414]`: `Full compose logs | Log toan bo 7 service chay cung luc | logs-compose.txt` khong con phu hop voi repo hien tai.
- Block `[433]`: `docker-compose.yml # Stack day du 7 service` can them ghi chu `file khai bao full stack, nhung branch hien tai chi co source A5`.
- File `reports/readiness-checklist.md` cung dang tick full 7 service healthy; khong nen dua vao bao cao neu khong co source/chay thuc te.

Khuyen nghi viet lai: "Nhom A5 demo standalone bang `docker-compose.analytics.yml`; `docker-compose.yml` la cau hinh tham chieu full product va can source day du tu cac nhom khac."

### 3.3. Evidence trong Word khong khop voi noi dung A5

Word co 5 anh nhung khong du minh chung cho A5:

- `image1.png`: logo Dai Nam.
- `image2.png`: docker compose ps full 7 service cu.
- `image3.png`: Swagger cua Access Gate, khong phai A5.
- `image4.png`: Swagger cua IoT Ingestion, khong phai A5.
- `image5.png`: Swagger cua Core Business, khong phai A5.

Can thay bang anh A5 moi:

- Cho `Hinh 1`: dung `reports/report-review/screenshots/01-a5-docker-compose-ps.png`.
- Cho `Hinh 2`: dung `reports/report-review/screenshots/02-a5-health.png`.
- Them hinh metrics: `reports/report-review/screenshots/03-a5-metrics.png`.
- Them hinh recent events: `reports/report-review/screenshots/04-a5-recent-events.png`.
- Them hinh logs MQTT: `reports/report-review/screenshots/05-a5-live-logs.png`.
- Neu muon giai thich vi sao khong chay full stack: dung `reports/report-review/screenshots/06-full-compose-build-failure.png`.

### 3.4. So lieu trong Word khong khop evidence moi

Word dang dung bo so lieu cu ngay 18/06/2026:

- `total_events = 2231`
- `sensor_events = 1584`
- `access_events = 647`
- `deny_rate = 43.8%`
- `low_battery_count = 12`

Evidence moi ngay 2026-06-29 co so lieu khac, vi container vua restart va bat dau dem lai tu dau:

- `reports/report-review/evidence/a5-health.json`: `total_events = 50`
- `reports/report-review/evidence/a5-metrics.json`: `sensor_events = 32`, `access_events = 18`, `access_deny_rate_percent = 33.3`

Phai chon 1 trong 2 cach:

1. Neu giu so lieu 2231 trong Word, phai dung lai bo evidence cu ngay 18/06/2026 va khong ghi de `reports/analytics-*.json`.
2. Neu dung evidence moi vua chup, phai cap nhat toan bo so lieu trong Word theo file moi.

Khong nen de Word ghi 2231 nhung file evidence trong repo lai la 50.

### 3.5. Ket luan ve A2 va A6 dang mau thuan

Trong Word co hai luong noi dung trai nhau:

- Block `[271]`, `[292]`, `[293]`, `[354]`: noi A2 camera va A6 core alert chua co/khong tang, camera/core = 0.
- Block `[488]`, `[489]`: bang muc do dat muc tieu lai ghi `Tich hop voi A2 Camera Stream | Hoan thanh` va `Tich hop voi A6 Core Business | Hoan thanh`.

Can sua thanh:

- A1 IoT: hoan thanh neu co sensor events.
- A3 Access: hoan thanh neu co access events.
- A2 Camera: "chua co evidence thuc te tai thoi diem demo" hoac "cho xac nhan publish topic".
- A6 Core: "chua co core_alert events trong evidence hien tai" neu `core_alerts = 0`.

Neu muon claim A2/A6 hoan thanh, can co anh/log/recent events co topic:

```text
smart-campus/events/camera
smart-campus/events/core-alert
```

## 4. Loi Noi Dung Va Ky Thuat Can Sua

### 4.1. Endpoint path khong thong nhat

Word block `[035]` ghi:

```text
/api/v1/metrics/{name}
```

Code va OpenAPI dung:

```text
/api/v1/metrics/{metric_name}
```

Can sua trong bao cao thanh `{metric_name}`.

### 4.2. Noi dung ma loi 503 khong dung voi code

Word block `[150]` noi:

```text
503 Service Error | MQTT broker disconnect
```

Nhung code `GET /health` luon return HTTP 200, chi doi field:

```json
"mqtt_connected": false
```

Can sua: bo dong 503 hoac ghi "khong tra 503; health van 200 nhung `mqtt_connected=false`".

### 4.3. Query param `limit` noi toi da 500 nhung code chua validate

OpenAPI va Word noi `limit` toi da 500, nhung code hien tai:

```python
def get_recent_events(limit: int = 20):
```

Chua dung `Query(20, ge=1, le=500)`. Neu muon report noi toi da 500, nen sua code. Neu giu code goc, sua bao cao thanh "limit la integer, mac dinh 20; service tra ve slice theo so limit".

### 4.4. Bang phan cong bi thieu ten thanh vien

Block `[340]` bang phan cong:

- Dong dau co `(Truong nhom)`.
- Ba dong sau cot `Thanh vien` bi trong.

Can dien du ten:

- Nguyen Thanh Trung
- Do Van Truong
- Pham Dinh Minh Truong
- Duong Trong Tuan

Neu khong dien, giang vien co the coi la phan cong khong minh bach.

### 4.5. Radmin VPN con placeholder

Block `[247]`:

```text
May demo: _____________ Radmin IP cua nhom: _____________ Network: _____________
```

Can dien that hoac xoa neu khong co Radmin evidence.

Block `[248]` con `<RADMIN_IP>`, `<A5_RADMIN_IP>`. Nen thay bang IP that hoac ghi "khong su dung Radmin trong lan test nay".

### 4.6. Don vi do am sai

Block `[337]`:

```text
Do am trung binh: 58.7°C - 64.0%
```

Sai don vi. Do am phai la `%`, khong phai `°C`.

Sua:

```text
Do am trung binh: 58.7% - 64.0%
```

### 4.7. Phu luc bi nhay chu cai

Phu luc co:

- A. Ma nguon chinh
- B. Cau truc thu muc project
- C. Luong du lieu chi tiet
- D. Chi tiet Postman Collection
- F. Moi truong phat trien

Thieu muc `E`. Nen doi `F. Moi truong phat trien` thanh `E. Moi truong phat trien`, hoac them muc E.

### 4.8. Bao cao noi "khong dan mat khau that" nhung repo co credential mau/that

Word block `[215]` ghi:

```text
MQTT_IOT_PASSWORD | Password HiveMQ (KHONG dan mat khau that vao bao cao)
```

Nhung repo co credential trong `.env.example` va default code `main.py`. Neu day la public GitHub, day la rui ro bao mat.

Khuyen nghi:

- Trong bao cao chi ghi `MQTT_IOT_PASSWORD=<configured in .env>`.
- Trong `.env.example` nen de placeholder, vi du `MQTT_IOT_PASSWORD=your_password_here`.
- Trong `main.py`, nen default password rong va bat doc tu env.

### 4.9. Anh Swagger trong Word la cua service khac

Neu muon co anh Swagger/OpenAPI, phai chup A5:

```text
http://localhost:8000/docs
```

Khong nen dung anh:

- Access Gate
- IoT Ingestion
- Core Business

Tru khi muc do dang noi ve tich hop lien nhom va caption ghi ro do la API cua doi tac.

### 4.10. `reports/logs-compose.txt` va `reports/readiness-checklist.md` khong phu hop voi branch hien tai

Hai file nay mo ta full 7 service. Neu branch nop bai chi la A5 standalone, nen:

- Khong dua vao bang evidence chinh.
- Hoac tach thanh "tai lieu tham khao full product, khong phai evidence A5 standalone".

## 5. Muc Can Bo Sung/Thay Anh Trong Word

Nen chen/toi thieu co cac anh sau:

1. Docker PS A5 standalone
   - File moi: `reports/report-review/screenshots/01-a5-docker-compose-ps.png`
   - Chen o muc `4.2 Dockerfile va Docker Compose` hoac `7.2`.

2. Health check A5
   - File moi: `reports/report-review/screenshots/02-a5-health.png`
   - Chen o muc `7.1`.

3. Metrics response A5
   - File moi: `reports/report-review/screenshots/03-a5-metrics.png`
   - Chen o muc `3.4 GET /api/v1/metrics` hoac `7.3`.

4. Recent events response
   - File moi: `reports/report-review/screenshots/04-a5-recent-events.png`
   - Chen o muc `GET /api/v1/events/recent` hoac `VII. Minh chung`.

5. Docker logs MQTT subscribe/received
   - File moi: `reports/report-review/screenshots/05-a5-live-logs.png`
   - Chen o muc `5.4 Ket qua test tich hop` hoac `6.3 Ket qua kiem thu`.

6. Full compose khong chay duoc do thieu source
   - File moi: `reports/report-review/screenshots/06-full-compose-build-failure.png`
   - Khong nen chen vao bao cao nop neu muon bao cao gon; nen dung de biet can sua text "full 7 service".

Anh can chup bo sung neu muon claim tich hop lien nhom:

- Radmin IP hoac may doi tac goi `http://<A5_IP>:8000/health`.
- Recent events co topic `smart-campus/events/camera` neu claim A2 hoan thanh.
- Recent events co topic `smart-campus/events/core-alert` neu claim A6 hoan thanh.
- Postman Collection Runner PASS neu bao cao noi Postman pass.

## 6. Huong Dan Chay Tren May Khac Nen Ghi Trong Bao Cao

Dung cho branch hien tai:

```powershell
git clone https://github.com/Tuanhimmnz/BTLDVKN.git
cd BTLDVKN
git checkout group-a5-analytics
Copy-Item .env.example .env
docker compose -f docker-compose.analytics.yml up -d --build
docker compose -f docker-compose.analytics.yml ps
curl.exe http://localhost:8000/health
curl.exe -H "Authorization: Bearer smart-campus-dev-token-2026" http://localhost:8000/api/v1/metrics
curl.exe -H "Authorization: Bearer smart-campus-dev-token-2026" "http://localhost:8000/api/v1/events/recent?limit=10"
docker compose -f docker-compose.analytics.yml logs --tail=120 analytics
```

Neu may khac trong LAN/Radmin goi vao may demo:

```text
http://<IP_MAY_DEMO_A5>:8000/health
```

Khong ghi lenh `docker compose up -d --build` full stack, tru khi repo da co day du:

```text
services/iot_ingestion
services/camera_stream
services/access_gate
services/ai_vision
services/analytics
services/core_business
services/notification
```

## 7. Goi Y Sua Nhanh Bao Cao

Thu tu sua nen lam:

1. Giu muc `4.5 Dashboard giao dien web`, nhung chen anh dashboard that tu `http://localhost:8000/dashboard`.
2. Sua toan bo claim "full 7 service/container/logs" thanh "A5 standalone" hoac bo neu khong co source day du.
3. Cap nhat bang evidence VII theo anh moi trong `reports/report-review/screenshots`.
4. Cap nhat so lieu theo evidence moi hoac phuc hoi evidence cu neu muon giu moc 2231 events.
5. Sua bang muc do dat muc tieu: A2/A6 khong nen de "Hoan thanh" neu counters bang 0.
6. Dien Radmin IP that hoac xoa cac placeholder.
7. Dien ten thanh vien trong bang phan cong.
8. Sua loi don vi do am va phu luc bi nhay chu cai.
9. Thay anh Swagger cua service khac bang anh A5 hoac doi caption cho dung.
10. Kiem tra lai `.env.example`/credential truoc khi nop public GitHub.

## 8. Ket Luan Review

Phan code A5 standalone hien chay duoc va co dashboard HTML. Loi lon nhat con lai nam o bao cao/evidence: bao cao dang claim full 7 service, so lieu 2231 events va anh Swagger cua service khac chua khop voi branch hien tai. Neu sua cac diem tren, bao cao se chat che hon va it bi giang vien bat loi hon.
