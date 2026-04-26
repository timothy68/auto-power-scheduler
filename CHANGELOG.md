# Changelog

Tất cả thay đổi đáng chú ý cho Auto Power Scheduler được ghi tại đây.

Format: [Keep a Changelog](https://keepachangelog.com/) | Versioning: [SemVer](https://semver.org/)

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
