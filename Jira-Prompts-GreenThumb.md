# JIRA PROMPTS/TEMPLATE — GREENTHUMB (7 TUẦN)

Mục tiêu file này: bạn **copy/paste 100%** để tạo Jira nhanh, đồng bộ với timeline 7 tuần + 2 prototype (PA1 Wi‑Fi + Cloud MQTT/HTTP).

---

## 0) Quy ước chung (dùng cho toàn dự án)

### 0.1. Tên sprint (7 tuần)
- `Sprint 0 - Week 1 (Setup & Planning)`
- `Sprint 1 - Week 2 (Prototype #1 - Device bring-up)`
- `Sprint 2 - Week 3 (Cloud ingest & DB)`
- `Sprint 3 - Week 4 (Mobile v1 & control)`
- `Sprint 4 - Week 5 (Prototype #2 - Calibration & stability)`
- `Sprint 5 - Week 6 (E2E Integration & QA)`
- `Sprint 6 - Week 7 (UAT, Demo, Report)`

### 0.2. Label gợi ý (để lọc nhanh)
Dùng 1–3 label/issue:
- `hw` (hardware), `fw` (firmware), `cloud`, `mobile`, `qa`, `doc`, `pm`, `security`, `integration`

### 0.3. Mẫu “Definition of Done” (DoD) cho issue
Một issue chỉ Done khi
- Có **evidence** (ảnh/log/video/link) trong Description hoặc comment
- Có **cách chạy lại/verify** (Steps to verify)
- Pass smoke test tối thiểu liên quan
- Không còn blocker và có owner rõ ràng

### 0.4. Mẫu Description chuẩn (copy vào mọi Feature/Task)
Dán mẫu dưới đây vào trường **Description**, rồi điền:

**Goal**
- (1 câu) mục tiêu business/technical

**Scope**
- In-scope:
  - 
- Out-of-scope:
  - 

**Dependencies**
- Upstream:
  - 
- Downstream:
  - 

**Acceptance Criteria (AC)**
- AC1:
- AC2:
- AC3:

**Steps to Verify**
1)
2)
3)

**Evidence**
- Link/ảnh/log:

---

## 1) PROMPT tạo 1 Feature (mẫu chung)
Dùng khi bạn bấm **Create → Feature** (như ảnh).

### 1.1. Feature — Summary (chuẩn đặt tên)
Dùng prefix sprint để đồng bộ timeline:
- `[S0] ...` hoặc `[S1] ...` … `[S6] ...`

Ví dụ format:
- `[S1] Prototype #1 - ESP32 bring-up + sensor read + Wi‑Fi`

### 1.2. Feature — Description (dán mẫu + điền nhanh)
Dán nguyên khối sau và chỉ sửa các dòng có dấu `(...)`:

**Goal**
- (Goal ngắn gọn, đo được)

**Scope**
- In-scope:
  - (liệt kê 3–6 đầu việc)
- Out-of-scope:
  - (1–3 thứ không làm trong sprint này)

**Dependencies**
- Upstream:
  - (issue nào cần xong trước)
- Downstream:
  - (issue nào sẽ phụ thuộc)

**Acceptance Criteria (AC)**
- AC1: (điều kiện pass/fail rõ)
- AC2: (điều kiện pass/fail rõ)
- AC3: (evidence bắt buộc)

**Steps to Verify**
1) (cách test nhanh)
2) (kỳ vọng)

**Evidence**
- Link/ảnh/log: (để trống rồi bổ sung sau)

---

## 2) PROMPT tạo 1 Task (mẫu chung)
### 2.1. Task — Summary
Format khuyến nghị:
- `[Sx][fw] ...` / `[Sx][cloud] ...` / `[Sx][mobile] ...` / `[Sx][qa] ...` / `[Sx][doc] ...`

Ví dụ:
- `[S2][cloud] MQTT ingest service - validate payload + persist DB`

### 2.2. Task — Description
Dán mẫu ở mục **0.4**.

---

## 3) PROMPT tạo 1 Bug (mẫu chung)
### 3.1. Bug — Summary
- `[BUG][Sx] (triệu chứng ngắn) - (khu vực)`

### 3.2. Bug — Description (mẫu)
**Observed**
- 

**Expected**
- 

**Steps to Reproduce**
1)
2)
3)

**Environment**
- Device/OS:
- App version:
- Firmware version:
- Network:

**Logs / Evidence**
- 

**Impact**
- (High/Med/Low) + lý do

---

# 4) BACKLOG CHUẨN THEO 7 SPRINT (COPY/PASTE TỪNG ISSUE)

Gợi ý: mỗi sprint tạo 3–6 Feature + 10–25 Task (tùy sức). Dưới đây là “đủ dùng” và bám sát báo cáo.

---

## SPRINT 0 — Week 1 (Setup & Planning)

### Feature 0.1
**Summary:** `[S0][pm] Setup project Jira + workflow + conventions`

**Description:**
**Goal**
- Thiết lập board/sprint + quy ước naming/label/DoD để vận hành ổn định 7 tuần

**Scope**
- In-scope:
  - Tạo 7 sprint (S0→S6)
  - Tạo label chuẩn (hw/fw/cloud/mobile/qa/doc/pm)
  - Tạo template Description/AC/Evidence
  - Tạo backlog khung theo milestone Prototype #1/#2/E2E/UAT
- Out-of-scope:
  - Tối ưu workflow phức tạp (chỉ cần To Do/In Progress/Done)

**Dependencies**
- Upstream:
  - None
- Downstream:
  - Tất cả sprint

**Acceptance Criteria (AC)**
- AC1: Có đủ 7 sprint và backlog khung
- AC2: Tất cả issue dùng được label + summary prefix
- AC3: Có 1 ảnh chụp board/backlog làm evidence

**Steps to Verify**
1) Mở backlog xem đủ sprint
2) Tạo thử 1 issue theo template

**Evidence**
- Link/ảnh/log:

**Tasks gợi ý (tạo Task riêng):**
- `[S0][pm] Create all sprints S0-S6`
- `[S0][pm] Create labels + naming convention page`
- `[S0][doc] Freeze FR/NFR + In/Out scope`

### Feature 0.2
**Summary:** `[S0][arch] Define PA1 architecture + MQTT/HTTP contract (v0)`

**Description:**
**Goal**
- Chốt contract dữ liệu để HW/FW/Cloud/Mobile làm song song

**Scope**
- In-scope:
  - Định nghĩa payload JSON (fields + units)
  - Quy ước topic MQTT hoặc endpoint HTTP
  - Quy ước deviceId + auth token (mức cơ bản)
  - Mock sample message
- Out-of-scope:
  - Bảo mật nâng cao (rotate token/audit log)

**Dependencies**
- Upstream:
  - None
- Downstream:
  - Sprint 1–3

**Acceptance Criteria (AC)**
- AC1: Có doc payload + 2 ví dụ message
- AC2: Cloud ingest mock nhận được message mẫu
- AC3: Evidence: link doc + screenshot test

**Steps to Verify**
1) Publish sample payload
2) Cloud mock log nhận đúng field

**Evidence**
- Link/ảnh/log:

---

## SPRINT 1 — Week 2 (Prototype #1 - Device bring-up)

### Feature 1.1
**Summary:** `[S1][fw] Prototype #1 - ESP32 bring-up + sensor read + Wi‑Fi`

**Description:**
**Goal**
- Thiết bị đọc sensor + kết nối Wi‑Fi ổn định, sẵn sàng gửi dữ liệu theo contract v0

**Scope**
- In-scope:
  - ESP32 boot ổn định
  - Đọc sensor (độ ẩm đất + temp/humidity nếu có)
  - Kết nối Wi‑Fi + reconnect cơ bản
  - Serialize payload JSON theo contract
- Out-of-scope:
  - Auto watering theo lịch (để sprint sau)

**Dependencies**
- Upstream:
  - `[S0][arch] Define PA1 architecture + MQTT/HTTP contract (v0)`
- Downstream:
  - Cloud ingest (S2)

**Acceptance Criteria (AC)**
- AC1: Kết nối Wi‑Fi ≤ 60s, có reconnect khi mất mạng
- AC2: Payload đúng field/unit, gửi được ra serial/log
- AC3: Evidence: ảnh/video demo + log 30 phút

**Steps to Verify**
1) Boot device, join Wi‑Fi
2) In log thấy payload mỗi X giây

**Evidence**
- Link/ảnh/log:

**Tasks gợi ý:**
- `[S1][fw] Read soil moisture sensor + basic smoothing`
- `[S1][fw] Read temp/humidity sensor (optional)`
- `[S1][fw] Wi‑Fi connect + reconnect`
- `[S1][fw] Build JSON payload (contract v0)`
- `[S1][qa] Smoke test checklist for Prototype #1`

### Feature 1.2
**Summary:** `[S1][hw] Prototype #1 - Power + pump wiring safe demo`

**Description:**
**Goal**
- Có mạch nguồn/bơm chạy demo an toàn để không “dồn rủi ro” đến cuối

**Scope**
- In-scope:
  - Wiring bơm + relay/driver
  - Test bật/tắt bơm manual (local)
  - Ghi nhận lỗi sụt áp/reset (nếu có)
- Out-of-scope:
  - Vỏ chống nước (để hướng phát triển)

**Acceptance Criteria (AC)**
- AC1: Bơm bật/tắt được 20 lần không reset
- AC2: Không quá nhiệt/không chập
- AC3: Evidence: video/ảnh + ghi chú test

---

## SPRINT 2 — Week 3 (Cloud ingest & DB)

### Feature 2.1
**Summary:** `[S2][cloud] MQTT/HTTP ingest + persist DB + basic validation`

**Description:**
**Goal**
- Cloud nhận dữ liệu từ device theo contract, lưu lại và truy vấn được

**Scope**
- In-scope:
  - MQTT broker hoặc HTTP endpoint
  - Validate payload (required fields)
  - Lưu DB (schema cơ bản)
  - API đọc latest readings
- Out-of-scope:
  - Rule engine nâng cao

**Acceptance Criteria (AC)**
- AC1: Nhận được payload thật từ Prototype #1
- AC2: Có DB record + API trả về latest
- AC3: Evidence: screenshot log + query

**Tasks gợi ý:**
- `[S2][cloud] Setup MQTT broker/HTTP endpoint (dev)`
- `[S2][cloud] DB schema + migrations (basic)`
- `[S2][cloud] Ingest service - validate + persist`
- `[S2][cloud] Read API - latest readings`
- `[S2][qa] Test ingest with sample payloads`

### Feature 2.2
**Summary:** `[S2][cloud] Alert rule (v0) + event log`

**Acceptance Criteria (AC)**
- AC1: Có ngưỡng độ ẩm thấp → tạo event
- AC2: Event lưu được, truy vấn được
- AC3: Evidence: event created from test payload

---

## SPRINT 3 — Week 4 (Mobile v1 & control)

### Feature 3.1
**Summary:** `[S3][mobile] Mobile v1 - onboarding + view readings + device list`

**Acceptance Criteria (AC)**
- AC1: Add device (deviceId/token) được
- AC2: Xem latest readings
- AC3: Evidence: video demo 1 luồng

**Tasks gợi ý:**
- `[S3][mobile] Add device screen + store credentials (basic)`
- `[S3][mobile] Readings screen (latest)`
- `[S3][mobile] Device list screen`

### Feature 3.2
**Summary:** `[S3][cloud+mobile] Manual pump control (v0) end-to-end`

**Acceptance Criteria (AC)**
- AC1: App gửi lệnh bật/tắt
- AC2: Cloud ghi nhận command + device nhận (hoặc mock)
- AC3: Evidence: log command + demo

---

## SPRINT 4 — Week 5 (Prototype #2 - Calibration & stability)

### Feature 4.1
**Summary:** `[S4][hw+fw] Prototype #2 - calibration + stability improvements`

**Acceptance Criteria (AC)**
- AC1: Hiệu chuẩn sensor (2 điểm hoặc quy ước) + ghi tài liệu
- AC2: Giảm reset do nguồn/bơm (bằng test lặp)
- AC3: Evidence: bảng kết quả test

**Tasks gợi ý:**
- `[S4][fw] Sensor calibration routine + store coefficients`
- `[S4][hw] Improve power stability (decoupling/driver)`
- `[S4][qa] Pump stress test + report`

### Feature 4.2
**Summary:** `[S4][cloud] Notifications (v0) via FCM (optional nếu kịp)`

**Acceptance Criteria (AC)**
- AC1: Alert event → push notification (basic)
- AC2: Evidence: screenshot notification

---

## SPRINT 5 — Week 6 (E2E Integration & QA)

### Feature 5.1
**Summary:** `[S5][integration] End-to-end integration (Device ↔ Cloud ↔ App)`

**Acceptance Criteria (AC)**
- AC1: Data flow ổn định: device publish → app view latest
- AC2: Manual control hoạt động end-to-end
- AC3: Evidence: demo video + logs

**Tasks gợi ý:**
- `[S5][qa] Integration test cases (E2E)`
- `[S5][cloud] Hardening: retries/timeouts/basic auth checks`
- `[S5][mobile] UX polish minimal for demo`

### Feature 5.2
**Summary:** `[S5][security] Basic security checklist (token, least privilege)`

**Acceptance Criteria (AC)**
- AC1: Không hardcode secret vào repo
- AC2: Token validation basic
- AC3: Evidence: checklist pass

---

## SPRINT 6 — Week 7 (UAT, Demo, Report)

### Feature 6.1
**Summary:** `[S6][qa] UAT + bugfix + release checklist (giả định)`

**Acceptance Criteria (AC)**
- AC1: Chạy UAT theo test cases đã viết
- AC2: Bug critical = 0, major giảm rõ
- AC3: Evidence: UAT report + screenshot

**Tasks gợi ý:**
- `[S6][qa] Execute UAT test suite + record results`
- `[S6][qa] Fix critical bugs (triage)`
- `[S6][pm] Sprint review deck/demo script`

### Feature 6.2
**Summary:** `[S6][doc] Final report pack (Ch1–Ch7 + appendix Jira)`

**Acceptance Criteria (AC)**
- AC1: Report đủ chương + hình/bảng
- AC2: Appendix Jira có ảnh board/sprint/issues/report
- AC3: Evidence: link file final

---

# 5) CHECKLIST “ĐỒNG BỘ TIMELINE ↔ JIRA” (để bạn không lệch)
- Mỗi issue Summary phải có `[Sx]` tương ứng tuần
- Due date của issue nằm trong tuần sprint
- Mỗi sprint có 1–2 Feature “milestone” (Prototype #1/#2/E2E/UAT)
- Cuối sprint chụp 1–2 ảnh: Board + Burndown/Report (làm Phụ lục A)

Hết.
