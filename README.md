# Auto Power Scheduler

Phần mềm Windows desktop quản lý lịch bật / tắt / wake máy tự động cho **kiosk, máy POS, máy treo quảng cáo, máy vận hành theo giờ cố định**.

**Tác giả**: LeNhu Software · [codeforwork.io.vn](https://codeforwork.io.vn/)

---

## Tải về

| Phiên bản | Tải | Hash |
|---|---|---|
| **Latest stable** | [Releases page](https://github.com/timothy68/auto-power-scheduler/releases/latest) | SHA256 trong release notes |

Mỗi release có 2 file:
- **`AutoPowerScheduler_Setup_v3.X.exe`** — bộ cài đặt (khuyến nghị cho deploy)
- **`AutoPowerScheduler.exe`** — binary đơn (chạy trực tiếp, cần quyền Administrator)

App tự kiểm tra phiên bản mới mỗi lần khởi động và hiển thị banner thông báo nếu có update — không tự cài đè.

## Cài đặt nhanh

1. Tải `AutoPowerScheduler_Setup_v3.X.exe` từ [Releases](https://github.com/timothy68/auto-power-scheduler/releases/latest)
2. Right-click → **Run as administrator**
3. Next → Finish
4. App tự cấu hình `powercfg RTCWAKE = 1` (bật wake timer cho cả AC + Battery)
5. Mở app → tab Lịch trình → click **Kiểm tra vận hành** → Probe Sleep + Probe Hibernate trước khi deploy production

Xem [HUONG_DAN_SU_DUNG_AUTO_POWER_SCHEDULER.md](HUONG_DAN_SU_DUNG_AUTO_POWER_SCHEDULER.md) cho hướng dẫn đầy đủ + [HUONG_DAN_NHANH_GUI_KHACH_HANG.md](HUONG_DAN_NHANH_GUI_KHACH_HANG.md) cho quick start cho khách hàng.

## Yêu cầu hệ thống

- Windows 10 (build 1809 trở lên) hoặc Windows 11
- Tài khoản có quyền Administrator
- BIOS / UEFI hỗ trợ Wake Timers (RTCWAKE) — app có Pre-flight check sẽ tự verify

## Tính năng chính

- Lịch wake / sleep / hibernate tự động theo ngày trong tuần
- Tự động đăng nhập Windows sau khi máy thức
- Bỏ qua ngày lễ / ngày nghỉ
- **Smart Power Mode** — tự detect Sleep S3 / Hibernate / Modern Standby trên từng máy
- **Wake Lockdown** — tắt nguồn wake không mong muốn (Maintenance, Fast Startup, NIC PM)
- Pre-flight check 21 bước + Real wake test
- Bug report tự đóng gói (diagnostic + log + config redacted)

Xem [CHANGELOG.md](CHANGELOG.md) cho lịch sử thay đổi từng version.

## Báo lỗi / Hỗ trợ

Trong app: tab Lịch trình → **Kiểm tra vận hành** → **📤 Gửi báo cáo lỗi**.

App tự tạo zip bundle (diagnostic + log + config redacted password) trên Desktop và mở email với template tới support. Anh chỉ cần kéo file zip vào email rồi gửi.

Hoặc liên hệ trực tiếp:
- Website: [codeforwork.io.vn](https://codeforwork.io.vn/)
- GitHub Issues: [Báo cáo lỗi](https://github.com/timothy68/auto-power-scheduler/issues)

## License

Phần mềm thương mại — yêu cầu license key. Liên hệ [codeforwork.io.vn](https://codeforwork.io.vn/) để mua license.

Source code không bao gồm trong public repo này (proprietary). Repo public chỉ chứa documentation + binary releases để khách hàng tải về và verify SHA256.

**Không được redistribute binary** đã tải về nếu không có thỏa thuận với LeNhu Software.
