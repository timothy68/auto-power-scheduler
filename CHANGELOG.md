# Changelog

Tất cả thay đổi đáng chú ý cho Auto Power Scheduler được ghi tại đây.

Format: [Keep a Changelog](https://keepachangelog.com/) | Versioning: [SemVer](https://semver.org/)

---

## [3.2.0] — 2026-05-19

### Added

- **Tab "Kiosk Boot Manager"** — tab thứ 5 trong GUI chính, quản lý kiosk boot trực tiếp từ app, không cần mở terminal:
  - **Status panel**: hiển thị realtime trạng thái running / down / disabled cho từng app, có nút Start / Stop per-app. Check song song nhiều app, tổng thời gian tối đa 8s.
  - **Task Scheduler control**: cài / gỡ / chạy ngay task khởi động kiosk từ GUI. Browse chọn script khởi động trực tiếp.
  - **Telegram alert**: nhập token + chat ID, test gửi thử, lưu vào config. Token hiển thị dạng `*` (masked).
  - **Log viewer**: xem 50 dòng cuối log boot, nút mở file gốc bằng trình xem mặc định.
  - **Config editor**: chỉnh config JSON trong dialog, validate trước khi lưu. Ghi atomic chống corrupt khi app bị kill.
  - **Export deploy pack**: đóng gói config + script khởi động thành `.zip` để IT roll-out hàng loạt cho hàng nghìn máy.

### Performance

- Tab Kiosk Boot không block UI — mọi subprocess, socket check, HTTP call đều chạy background.
- Zero CPU khi idle: status chỉ refresh khi user bấm "Làm mới", không có polling timer.

### Compatibility

- Config Kiosk Boot tách biệt hoàn toàn với config power scheduler — không xung đột.
- Auto-update từ 3.1.3 → 3.2.0 hoạt động bình thường.

---

## [3.1.3] — 2026-05-15

### Added

- **App không còn freeze** trong các thao tác nặng (Apply schedule, Lockdown, Quick Action, mở View dialog, Delete schedule): button hiển thị "🔄 Đang xử lý..." kèm spinner, modal busy có animation cho ops >2s. Triệt tiêu hoàn toàn label "Not Responding" của Windows trước đây xuất hiện khi UI freeze 1-5s.
- **Time hint động cho mọi ô giờ** — gõ `15:30` sẽ hiện hint "3 giờ chiều" (VN) hoặc "3 PM" (EN), gõ sai format hiện cảnh báo "⚠ Định dạng giờ chưa đúng". Apply cho Tab Cơ Bản, Wizard step 1, Auto-update window. Labels thêm "(24h)" để rõ ràng.

### Changed

- Icon button "Tắt ngay" đổi `⏻` → `🔴` (một số máy thiếu glyph U+23FB nên render thành ô vuông). Đồng bộ visual với "💤 Ngủ ngay" và "🌙 Ngủ đông ngay".

### Performance

- Mọi heavy op (subprocess, registry, file I/O lớn, schtasks, powercfg, network) chạy background — UI thread luôn free cho user interaction.
- Quick Action (Sleep/Hibernate/Shutdown): pre-flight check chạy background → button feedback ngay khi click, không đợi 0.5-1s như trước.
- 5 view dialog (Tasks / Diagnostics / Power compatibility / Lockdown status / Log) mở ngay với thông báo "🔄 Đang tải dữ liệu...", refill khi data về — không freeze UI 1-3s như trước.

### Compatibility

- Auto-update từ 3.1.2 → 3.1.3 hoạt động bình thường.
- Config 3.1.2 dùng OK với 3.1.3, không cần migration.

---

## [3.1.2] — 2026-05-15

### Fixed (CRITICAL)

- **Popup "Máy đã khởi động" xuất hiện không cần thiết**: trước đây mỗi lần máy resume từ Modern Standby (rất phổ biến trên kiosk hiện đại) đều trigger popup full-screen 12s, dù user không yêu cầu. Giờ chỉ hiện popup khi đúng cửa sổ ±15 phút quanh giờ wake đã config và hôm nay chưa hiện popup.
- **Kẹt phím + Start menu nhảy lên** sau wake: cơ chế wake display cũ giả lập phím Ctrl+Esc, Enter, NumLock, Shift × 5 — có thể trigger Start menu, submit form đang mở, đảo đèn NumLock, hoặc kích hoạt Sticky Keys. Giờ chỉ dùng chuyển động chuột 0 pixel — an toàn tuyệt đối, không ảnh hưởng UI đang mở.
- **Config bị reset về default mỗi lần load**: validate giờ chỉ accept format `HH:MM` nhưng nội bộ lưu `HH:MM:SS` → toàn bộ schedule bị reject → user mất lịch âm thầm. Sửa: accept cả 2 format.
- **Resume monitor false positive khi GUI bị block**: timer detect resume có thể trigger sai khi modal dialog mở >60s, GC pause, disk lag. Sửa: dùng dual-clock check (wall time + tick count) — chỉ trigger khi thực sự có sleep / hibernate.

### Fixed (HIGH)

- **Ghi config không atomic**: nếu app bị kill (Windows shutdown, force terminate) đúng lúc đang write → file rỗng / JSON parse error → app reset về default → mất lịch. Giờ dùng tempfile + fsync + atomic replace (an toàn ngay cả khi cắt nguồn).
- **Log rõ hơn khi config corrupt**: trước đây silent swallow exception → debug khó. Giờ ghi rõ lý do parse failed trong log.
- **Graceful shutdown không lặp**: thêm guard tránh cleanup chạy 2 lần khi Windows shutdown trigger nhiều event liên tiếp.
- **Runner mode rút ngắn thời gian sống**: scheduled task process trước tồn tại ~11s sau khi wake xong (vì sleep stabilize). Giờ exit ngay khi đủ điều kiện, tránh treo background.

### Changed — UI Refresh

- **Tab "Lịch Bật/Tắt Máy" cũ tách thành 4 tab semantic**:
  - **Cơ Bản**: Probe banner + Wake + Shutdown + Apply
  - **Nâng Cao**: Autologin + Postwake + Appsec + Lockdown + Auto-update + Apply
  - **Ngày Lễ**: giữ nguyên
  - **Chẩn Đoán**: Quick Action + Test cycle + System test + 6 utility button (View tasks / Diag / Compat / Log / Help / Delete)
- **Color scheme nhất quán** theo ngữ nghĩa: Wake = XANH LÁ, Shutdown = ĐỎ, Security (Autologin/Appsec/Lockdown) = CAM, Settings (Postwake/Auto-update) = XANH DƯƠNG. Trước đây có lúc Lockdown hiện màu xanh dương khiến user nhầm là power-off.
- **Status bar 2-line history**: line 1 hiển thị status mới nhất (bold), line 2 hiển thị 2 dòng cũ với timestamp (xám) — không mất context khi status thay đổi nhanh.
- **BIOS section → Help dialog on-demand**: trước chiếm chỗ trong tab Lịch mỗi lần mở app. Giờ vào qua nút "❓ Trợ giúp" trong Chẩn Đoán, dialog có 3 phần (Quick tips + BIOS setup + Support contact).
- **Notification balloon thay vì modal popup** cho 4 thao tác phụ (autologin OK, appsec OK, copy HWID OK) — không phải dismiss bằng tay nữa.

### Added

- **Setup Wizard 3 bước** cho user lần đầu mở app:
  - Step 1: setup giờ wake / shutdown + chọn ngày trong tuần
  - Step 2: probe máy để verify wake mode (skip-able)
  - Step 3: confirm summary + autostart + Apply
  - Tự bỏ qua nếu user legacy đã có schedule (không bị làm phiền).
- **Validate config strict** cho mọi field (bool, days list, giờ regex, power mode enum, ngôn ngữ, paths, delays). Sai → reset về default + log warning.

### Compatibility

- Backward compat hoàn toàn với config 3.1.1: không cần migration.

---

## [3.1.1] — 2026-05-10

### Fixed (CRITICAL)

- **Bug shutdown hang trên Windows 11**: app không trả lời khi user click Restart / Shutdown qua Start menu → Windows hiện dialog "This app is preventing restart" và user phải bấm "Force restart". Sửa: app giờ consent shutdown ngay lập tức và thoát sạch, không còn block Windows.

### Added

- **Quick Action panel** trên Tab Schedule: 3 button "💤 Ngủ ngay / 🌙 Ngủ đông ngay / ⏻ Tắt ngay" cho phép sleep / hibernate / shutdown ngoài giờ lịch mà không ảnh hưởng lịch ngày sau (tránh dùng Start menu Windows vì có thể skip cycle wake hôm sau).
- **Pre-flight check** trước mỗi quick action: verify Fast Startup OFF, Hibernate available, task wake ngày sau enabled + có NextRunTime. Cảnh báo cho user xem trước khi proceed.
- **Recommended button highlight**: app tự highlight button phù hợp với máy (Modern Standby → Hibernate, desktop S3 → Sleep) bằng viền cam dày + suffix "★". User đổi power mode → highlight cập nhật ngay không cần Save.
- **Cảnh báo khi shutdown từ Windows Start menu**: nếu app phát hiện lịch ngày sau có vấn đề, hiện balloon notification cảnh báo (không block shutdown).
- **Auto-update silent** — quan trọng cho deploy hàng loạt 6000 kiosk: app tự download bản mới (verify SHA256 chống MITM) + cài vào cửa sổ giờ rảnh (mặc định 03:00-05:00). UI section "Tự động cập nhật" có toggle, chỉnh window time, nút "Kiểm tra ngay" / "Cài ngay (force)". Khi tắt auto-update → fallback hiện banner cho user tự click Download. Chỉ install nếu release notes có dòng `SHA256: <64hex>`.
- **Self-test mode**: chạy `AutoPowerScheduler.exe --self-test` → kiểm tra 11 điểm critical (shutdown handler, task batch query, tray, cache, quick action, auto-update, admin rights, modules, log size). Build pipeline tự chạy hậu kiểm, cảnh báo nếu có FAIL. Chạy được không cần quyền Administrator.

### Performance

- **Cache trạng thái Wake Lockdown** TTL 60s: trước mỗi lần check mất 1-15s vì enumerate ALL scheduled tasks qua PowerShell. Cache hit instant — Diagnostics dialog mở lần 2 trong 60s sẽ instant.
- **Tray icon recycle**: trước đây mỗi lần hide / show tạo lại icon → leak resource theo thời gian. Giờ reuse — giảm RAM khi app chạy lâu.
- **Adaptive resume monitor**: timer chạy 10s khi UI visible, 60s khi minimized vào tray (giảm 6x CPU baseline).
- **Batch query task**: gộp 3 lần query Wake/Shutdown/Autostart thành 1 call (1.5-2x nhanh hơn).
- **Notification balloon giảm thời gian sống** 5s → 2s — release RAM nhanh hơn vì app gọi notification 11 chỗ.
- **Log viewer tail-read**: chỉ đọc 500KB cuối thay vì full file — không còn spike RAM 30-50MB khi mở log lớn.

### Compatibility

- Vẫn yêu cầu Windows 10 (1809+) hoặc Windows 11.

---

## [3.1.0] — sắp release

### Changed (BREAKING)

- **License enforcement HARD-LOCK**: trial 14 ngày hết hạn → app KHÔNG còn chạy được, hiển thị dialog yêu cầu mua license hoặc liên hệ. Trước đây trial expired chỉ hiện banner cảnh báo, app vẫn chạy.
- **Runner mode (Task Scheduler trigger) cũng bị block**: nếu license expired, các tác vụ wake/sleep/hibernate/shutdown qua Task Scheduler sẽ KHÔNG thực thi (log + exit). Đảm bảo enforcement triệt để.

### Added

- Modal dialog hiển thị Machine ID + thông tin liên hệ:
  - Email: lenhu.coder@gmail.com
  - Hotline: 090 112 7849
  - Website: codeforwork.io.vn
- 4 nút action trong lock dialog: Nhập license, Copy Machine ID, Gửi email mua license, Đóng app
- Email mailto template tự động điền HWID + form mua license

### Fixed

- "Buy license" button trong dialog About bây giờ trỏ đúng `codeforwork.io.vn` (trước là placeholder `your-website.com`)

---

## [3.0.0] — 2026-04-27

### 🎯 Phiên bản đầu tiên public

Bản release ổn định đầu tiên cho khách hàng triển khai kiosk.

### Tính năng chính

- **Lịch wake / sleep / hibernate** tự động theo ngày trong tuần
- **Tự động đăng nhập Windows** sau khi máy thức từ Sleep / Hibernate
- **Khởi động cùng Windows** (autostart, hỗ trợ Win10 + Win11)
- **Bỏ qua ngày lễ / ngày nghỉ** với holiday API
- **Smart Power Mode** — app tự detect Sleep S3 / Hibernate / Modern Standby và chọn phương án phù hợp cho từng máy
- **Wake Lockdown** — tắt các nguồn wake không mong muốn (Maintenance, Fast Startup, NIC Power Management, scheduled tasks khác)
- **Pre-flight check 21 bước** — verify hệ thống trước khi deploy
- **Real wake test** — máy tự sleep + tự thức để verify hardware RTC
- **Quick cycle test** — test full cycle 5 phút bằng 1 click
- **Bug report bundle** — tự đóng gói diagnostic + log + config (đã redact password) gửi support

### Bảo mật

- HMAC-SHA256 license verification
- SHA256 verify khi auto-update download (chống MITM)
- Single-instance protection
- Run as Administrator required

### Compatibility

- Windows 10 (1809+)
- Windows 11
- Architecture: x64

---

## Format các version sau

```
## [X.Y.Z] — YYYY-MM-DD

### Added
- Tính năng mới

### Changed
- Thay đổi behavior

### Fixed
- Bug fixes

### Security
- Security improvements

### Deprecated
- Tính năng sắp bị bỏ
```
