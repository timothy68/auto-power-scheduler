# HUONG DAN SU DUNG AUTO POWER SCHEDULER

## 1. Muc dich cua phan mem

Auto Power Scheduler dung de:

- Hen gio bat may tu dong khi may dang Sleep hoac Hibernate
- Hen gio tat may hoac dua may vao Hibernate
- Tu dong bo qua lich vao ngay le, ngay nghi
- Tu dong dang nhap Windows sau khi khoi dong lai
- Tu khoi dong cung Windows
- Kiem tra va tu dong canh bao khi Task Scheduler bi lech cau hinh

Phan mem phu hop cho kiosk, may POS, may treo quang cao, may van hanh theo gio co dinh.

## 2. Yeu cau he thong

- Windows 10 hoac Windows 11
- Tai khoan co quyen Administrator khi cau hinh
- Cho tinh nang bat may tu dong:
  BIOS/UEFI phai ho tro Wake Timers va may phai duoc dua vao Sleep/Hibernate dung cach
- De tu dong dang nhap:
  Phai co mat khau Windows va moi truong su dung phai an toan

## 3. Cac file trong bo phan mem

Phan mem phan phoi qua GitHub Releases, moi release co 2 file:

- `AutoPowerScheduler_Setup_v3.X.exe`: bo cai dat (khuyen nghi cho deploy hang loat)
- `AutoPowerScheduler.exe`: binary don (chay truc tiep, can quyen Administrator)

Kem theo trong repo public:

- `HUONG_DAN_SU_DUNG_AUTO_POWER_SCHEDULER.md`: tai lieu huong dan day du (file nay)
- `HUONG_DAN_NHANH_GUI_KHACH_HANG.md`: tai lieu nhanh cho khach hang cuoi
- `CHANGELOG.md`: lich su thay doi tung phien ban
- `README.md`: gioi thieu va link tai

## 4. Cach cai dat va khoi dong

Khuyen nghi: dung bo cai `AutoPowerScheduler_Setup_v3.X.exe`.

1. Tai bo cai tu trang [Releases](https://github.com/timothy68/auto-power-scheduler/releases/latest)
2. Nhap chuot phai vao file `.exe`, chon `Run as administrator`
3. Theo wizard Next → Finish
4. Mo app tu Start Menu hoac Desktop shortcut (luon chay bang quyen Administrator)
5. Thiet lap lich can dung roi bam `Luu & Ap dung`

Truong hop chay binary don (khong qua bo cai), copy `AutoPowerScheduler.exe` vao may dich roi nhap chuot phai → `Run as administrator`.

## 5. Cau hinh lan dau cho kiosk

Day la trinh tu nen dung khi setup may moi:

1. Mo app bang quyen Administrator
2. Cau hinh `Bật máy tự động`
3. Cau hinh `Tắt máy tự động`
4. Bat `Khởi động ngầm cùng Windows` neu can app tu chay lai sau khi dang nhap
5. Bat `Tự động đăng nhập` neu may can tu vao desktop sau reboot
6. Vao tab `Ngày lễ / Nghỉ` neu can bo qua lich vao ngay le
7. Bam `Lưu & Áp dụng`
8. Bam `Kiểm tra hệ thống`
9. Reboot thu nghiem 1 lan

## 6. Huong dan tung chuc nang

### 6.1. Lich bat may tu dong

Muc `BẬT MÁY TỰ ĐỘNG` dung de danh thuc may tu Sleep/Hibernate.

Thuc hien:

1. Tich `Kích hoạt tự động thức / bật máy`
2. Chon gio thuc
3. Chon cac ngay trong tuan
4. Bam `Lưu & Áp dụng`

Luu y:

- Chuc nang nay khong thay the BIOS Power On after AC loss
- May phai dang o Sleep/Hibernate, khong phai da tat nguon hoan toan
- Windows phai cho phep `Allow wake timers`

### 6.2. Lich tat may tu dong

Muc `TẮT MÁY TỰ ĐỘNG` dung de tat may hoac dua may vao Hibernate.

Thuc hien:

1. Tich `Kích hoạt tự động tắt / hibernate máy`
2. Chon gio tat
3. Chon cac ngay trong tuan
4. Neu muon dong ung dung dang mo, tich `Buộc đóng ứng dụng đang chạy`
5. Bam `Lưu & Áp dụng`

Nguyen tac:

- Neu lich bat may dang bat, app uu tien `Hibernate` de sau do co the wake lai
- Neu khong dung lich bat may, app se tat may binh thuong

### 6.3. Khoi dong cung Windows

Muc `Khởi động ngầm cùng Windows` dung de app tu mo lai sau khi dang nhap Windows.

Khi bat muc nay:

- App tao task `AutoPowerScheduler_Autostart`
- Sau dang nhap, app se tu chay lai
- Neu phat hien task bi lech duong dan hoac tat, app se canh bao va co the tu dong dong bo lai

### 6.4. Tu dong dang nhap Windows

Muc `TỰ ĐỘNG ĐĂNG NHẬP` dung khi kiosk can tu vao desktop ma khong can nguoi thao tac.

Thuc hien:

1. Tich `Tự động đăng nhập khi Windows khởi động`
2. Nhap mat khau Windows
3. Bam `Áp dụng`
4. Reboot may de test

Luu y bao mat:

- Mat khau duoc luu trong Windows Registry
- Chi dung tren may kiosk dat tai dia diem an toan
- Khong khuyen nghi cho may van phong co du lieu nhay cam

### 6.5. Ngay le va ngay nghi

Tai tab `Ngày lễ / Nghỉ`, app co 2 cach:

- Tai tu dong ngay le Viet Nam tu Internet
- Them ngay nghi tuy chinh thu cong

Neu bat `Bỏ qua lịch bật / tắt máy vào ngày lễ và ngày nghỉ`, app se khong thuc hien wake/shutdown vao nhung ngay nay.

## 7. Cac nut chuc nang quan trong

### `Lưu & Áp dụng`

- Ghi cau hinh moi
- Tao hoac cap nhat Task Scheduler
- Chi luu cau hinh khi cac task tao thanh cong

### `Xem lịch hiện tại`

- Hien thong tin 3 task:
  - Wake
  - Shutdown
  - Autostart

### `Kiểm tra hệ thống`

Dung de kiem tra nhanh:

- App dang chay o che do nao
- Thu muc du lieu
- Trang thai auto-login
- Trang thai va ket qua chay cua cac task trong Task Scheduler

Day la nut can dung dau tien khi khach bao loi.

### `Lịch sử (Log)`

- Hien file log hoat dong cua app
- Huu ich khi can truy vet kiosk nao da wake, shutdown, skip holiday, auto-sync

### `Xóa tất cả lịch`

- Xoa cac lich Wake va Shutdown
- Khong xoa danh sach ngay le

### `🧪 Test cycle nhanh (5 phut)` (moi tu v3.0)

- Tu dong fill: shutdown = bay gio + 3 phut, wake = bay gio + 8 phut
- Auto Apply va kiem tra NextRun thuc te
- Dung de verify wake hardware tren may moi truoc khi deploy production
- Banner xanh = wake mode da verify, banner cam = 1 mode FAIL, banner do = ca 2 FAIL

### `📤 Gửi báo cáo lỗi` (moi tu v3.0)

Nam trong dialog `Kiem tra van hanh`. Khi gap loi:

1. Bam nut nay → app tu tao file zip tren Desktop voi:
   - diagnostic.txt (bao cao day du he thong)
   - auto_power_log.txt (log file)
   - config_redacted.json (cau hinh, da xoa password)
   - lockdown_audit.json (rollback info, neu co)
   - summary.txt (OS, hostname, version)
2. App mo Explorer tro vao file zip + mo email moi voi noi dung soan san den support
3. User keo file zip vao email roi gui

Khong gui password - he thong tu redact.

### Banner trang thai probe (tab Co Ban)

Hien o dau tab, cho biet wake mode da verify chua:

- **Vang** `⚠ Chua verify wake mode` — chay `Kiem tra he thong` truoc khi deploy
- **Xanh la** `✓ Wake mode da verify: Sleep/Hibernate` — san sang deploy
- **Cam** `⚠ Sleep ✓, Hibernate ✗ FAIL` — chi mode da PASS hoat dong, app tu fallback
- **Do** `✗ Ca Sleep + Hibernate FAIL` — may KHONG ho tro wake tu dong, khong nen deploy

## 8. Quy trinh kiem tra sau khi setup

Sau khi cau hinh xong, nen test theo thu tu sau:

1. Bam `Kiểm tra vận hành` → Probe Sleep S3 va Probe Hibernate (xac nhan banner xanh hoac cam)
2. Bam `🧪 Test cycle nhanh (5 phut)` → may phai tu sleep sau ~3 phut va tu thuc sau ~5 phut
3. Bam `Xem lịch hiện tại` → kiem tra NextRun cua Wake va Shutdown trung ngay (khong bi day sang Sunday tuan sau)
4. Bam `Kiểm tra hệ thống` → tat ca PASS hoac chi WARN khong critical
5. Reboot may → kiem tra:
   - may co tu dang nhap hay khong
   - app co tu chay lai hay khong
   - man hinh kiosk co hien dung hay khong
6. Setup lich production thuc te → cho test 1 cycle ngay-dem hoan chinh

## 9. Huong dan xu ly su co

### Hien tuong: App khong tu mo lai sau reboot

Kiem tra:

- Da bat `Khởi động ngầm cùng Windows` chua
- Nut `Kiểm tra hệ thống` co bao loi `Autostart` khong
- Co phan mem bao mat chan task hoac chan UAC khong

### Hien tuong: May khong tu dang nhap

Kiem tra:

- Da bat `Tự động đăng nhập` chua
- Mat khau Windows da nhap dung chua
- Da reboot lai may sau khi ap dung chua
- Tai khoan Windows co bi doi mat khau khong

### Hien tuong: May khong tu bat

Kiem tra:

- May dang Sleep/Hibernate hay da tat nguon hoan toan
- BIOS/UEFI co ho tro wake timer khong
- Windows co bat `Allow wake timers` khong
- Task `Wake` co ton tai khong

### Hien tuong: PowerShell nhay len lien tuc

Day la loi da duoc sua trong ban nang cap moi. Neu van gap:

1. Dong app
2. Dung ban `.exe` moi nhat
3. Bat lai app
4. Bam `Kiểm tra hệ thống`
5. Neu van loi, gui file log va anh chup man hinh lai

### Hien tuong: Kiem tra he thong bao task loi

Xu ly:

1. Bam `Đồng bộ ngay` neu app hien banner canh bao
2. Hoac bo tick va bat lai muc tinh nang do
3. Bam `Lưu & Áp dụng`
4. Kiem tra lai bang `Kiểm tra hệ thống`

## 10. Khuyen nghi cho trien khai 6000 kiosk

De giam loi van hanh, nen thong nhat:

- Cung 1 phien ban Windows
- Cung 1 ten tai khoan van hanh hoac quy uoc tai khoan
- Cung 1 chinh sach Power Options
- Cung 1 quy trinh setup
- Cung 1 bo test sau cai dat

Nen bo sung quy trinh support:

1. Yeu cau ky thuat vien bam `Kiểm tra hệ thống`
2. Chup man hinh bao cao
3. Mo `Lịch sử (Log)`
4. Gui log va trang thai cho bo phan ho tro

## 11. Goi y cach gui huong dan cho khach hang

Neu gui cho khach hang cuoi, nen tach thanh 2 muc:

- Ban huong dan ngan 1 trang:
  chi cach mo app, chon gio, bam `Lưu & Áp dụng`, reboot test
- Ban ky thuat day du:
  su dung file huong dan nay cho ky thuat vien

Neu Anh muon, co the lam them:

- 1 file PDF huong dan co anh chup man hinh
- 1 ban huong dan 1 trang de gui Zalo/Email
- 1 checklist setup kiosk de ky thuat vien tick tung buoc

## 12. Checklist giao cho khach

Truoc khi ban giao, can xac nhan:

- App mo duoc bang quyen Admin
- Wake task da tao
- Shutdown task da tao
- Autostart task da tao
- Auto-login da test thanh cong neu co su dung
- Reboot 1 lan khong loi
- Sleep/Hibernate va wake 1 lan khong loi
- `Kiểm tra hệ thống` khong con canh bao
- Log co ghi nhan hoat dong binh thuong

## 13. Thong tin bo phan ho tro

Khi bao loi, nen gui:

- Anh chup man hinh `Kiểm tra hệ thống`
- Anh chup man hinh `Xem lịch hiện tại`
- Noi dung `Lịch sử (Log)`
- Mo ta ro may dang gap o buoc nao:
  - khong tu dang nhap
  - khong tu mo app
  - khong wake
  - khong shutdown
  - man hinh khong hien

