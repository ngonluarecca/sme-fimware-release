# SME-02 | Kho Phát Hành Firmware OTA Chính Thức

Repository này **chỉ chứa các file nhị phân (`.bin`) và cấu hình `manifest.json`** phục vụ cập nhật từ xa (OTA) cho công tơ thông minh **SME-02**. Không chứa mã nguồn gốc.

## Cách Sử Dụng Trên Giao Diện Web SME-02
1. Truy cập giao diện quản trị công tơ SME-02 -> Tab **Hệ Thống (OTA)**.
2. Dán đường dẫn repo này vào ô **Kho Phát Hành Firmware OTA**:
   ```
   https://github.com/ngonluarecca/sme-fimware-release
   ```
3. Nhấn **Tải DS Phiên Bản** -> Chọn phiên bản mong muốn -> Bấm **Kiểm Tra Dung Lượng** -> Nhấn **Tải & Cập Nhật (FW + System)**.

---

## [NOTES] Lịch Sử Phiên Bản & So Sánh Tính Năng Mới

### v1.0.4 (2026-11-14) - Salt: `v1` | FW: 1608 KB | System FS: 128 KB
- [UI:auth] Bổ sung hỗ trợ phím Enter và nút 'Go/Done' trên bàn phím ảo di động để gửi form và đăng nhập tức thì trên toàn bộ các màn hình (Login, Đổi mật khẩu, AP Mode, Cài đặt Wi-Fi, Tài khoản, Biểu giá, MQTT, OTA).
- [UI:auth] Triển khai kiến trúc phiên đăng nhập bảo mật đa thiết bị (Per-Client Session Token qua X-Auth-Token), chặn triệt để rò rỉ quyền vào Incognito/máy khác và bổ sung nút Đăng Xuất.
- [UI:account] Tích hợp API GET/POST /api/v1/account tải và cập nhật động Tên tài khoản (Username) và Mật khẩu quản trị, xóa bỏ hoàn toàn giá trị cứng admin.
- [UI:wifi] Nâng cấp nút ẩn/hiện mật khẩu (👁️ <-> 🙈) phản hồi trực quan và nạp mật khẩu đã lưu từ NVS giúp xem/chỉnh sửa mật khẩu Wi-Fi dễ dàng.
- [UI:dashboard] Xóa bỏ 100% dữ liệu mẫu mockup tĩnh: liên kết thời gian thực các thẻ Hôm qua, Tháng trước, Dòng đỉnh và huy hiệu trạng thái Header (RTC, Wi-Fi, MQTT).
- [Backend] Bổ sung API GET/POST /api/v1/tariff và GET/POST /api/v1/mqtt quản lý động toàn bộ biểu giá điện bậc thang, chu kỳ chốt số và thông số kết nối MQTT/API.
- [UI:ota] Bổ sung ô nhập và lưu đường dẫn cập nhật Firmware OTA từ xa (URL .bin) vào bộ nhớ NVS kèm cơ chế nạp tự động qua HTTPClient.
- [UI:license] Thêm nút ẩn/hiện mật khẩu (👁️) cho trường License Key, hiển thị MAC thật và liên kết API GET /api/v1/license đồng bộ bản quyền.
- [Backend] Bổ sung API GET /api/v1/wifi tải danh sách 3 SSID đã lưu và API GET/POST /api/v1/ota/url quản lý đường dẫn nạp firmware từ xa.
- [UI:ap] Tích hợp màn hình theo dõi và trả về trực tiếp IP Router cấp (LAN IP) ngay trên giao diện 192.168.4.1 kèm liên kết mDNS và nút bấm mở trang quản trị.
- [Backend] Đặt Hostname ESP32 đồng bộ theo tên AP (SME_<MAC>), tích hợp mDNS responder động duy nhất (http://sme-<mac>.local) và giữ SoftAP chạy thêm 90s để báo IP cho client trước khi tắt.
- [Backend] Triển khai cơ chế Wi-Fi Dual-Mode WIFI_AP_STA kết nối song song: tự động chạy ngầm kết nối lại Router mỗi 30s mà không gián đoạn đo đếm Core 1 hay reset chip.
- [UI:onboarding] Triển khai hoàn chỉnh State Machine 4 màn hình: Cài đặt AP Wi-Fi (192.168.4.1) ➔ Đăng nhập LAN ➔ Bắt buộc đổi mật khẩu Admin ➔ Cổng khóa License (khóa API, hiện MAC, kích hoạt, reboot, factory reset) ➔ Full Web.
- [UI:dashboard] Xóa bỏ 100% dữ liệu mẫu mockup tĩnh trên Dashboard, khởi tạo số đo mặc định 0.0 và cập nhật thời gian thực từ PZEM-004T v3.
- [UI:system] Bổ sung nút "Khởi Động Lại (Reboot)" bên cạnh nút "Khôi Phục Cài Đặt Gốc" trong Full Web và loại bỏ hoàn toàn trường email.
- [Backend] Khóa dữ liệu API Telemetry và WebSocket khi thiết bị chưa kích hoạt bản quyền License Key hợp lệ.
- [Fix] Khắc phục lỗi PlatformIO thiếu cấu hình board_build.filesystem = littlefs dẫn đến nạp nhầm spiffs.bin và LittleFS tự format trắng.
- [UI:fs] Tích hợp bộ nạp giao diện nhúng PROGMEM Gzip dự phòng (embedded_ui.h) đảm bảo luôn hiển thị 100% giao diện kể cả khi chưa nạp LittleFS.
- [Chore] Bổ sung script flash_all.bat nạp tự động trọn gói 2 bước: Firmware + LittleFS trong 1 lần chạy duy nhất.
- [UI:ota] Bổ sung lựa chọn radio nạp Firmware / Filesystem OTA kèm thanh tiến trình và huy hiệu Chip ID F2124B00 trên giao diện web.
- [Backend] Tích hợp bộ xử lý OTA đa phân vùng: U_FLASH cho Firmware .bin, U_SPIFFS cho Filesystem .bin, và nạp trực tiếp file tĩnh (.html) vào LittleFS.
- [Chore] Đưa thư mục sme-PWAsmartconfig-esp32 vào trong ESP32-SME, cấu hình .gitignore cách ly hoàn toàn hai repository độc lập.
- [UI:fs] Tích hợp form tải trực tiếp file giao diện vào LittleFS qua Web (192.168.4.1) và script upload_fs.bat nạp nhanh qua USB.
- [Chore] Khởi tạo tài liệu README.md toàn diện về phần cứng, pinout, máy trạng thái nút bấm và cập nhật quy tắc đồng bộ README trong AGENTS.md.
- [Backend] Hoàn thiện trọn bộ mã nguồn firmware SME-02: Driver PZEM-004T v3 Modbus RTU, DS3231 RTC, Button FSM (GPIO 23), 3 LED Controller, NVS Storage, Multi-SSID Wi-Fi, WebServer, WebSocket 8084 và MQTT 1884.
- [Perf] Tối ưu hóa phân vùng min_spiffs (Flash 66%, RAM 16%), chạy FreeRTOS đa lõi không nghẽn và loại bỏ hoàn toàn cảnh báo biên dịch.
- [Backend] Cập nhật app_config.h và kiến trúc FreeRTOS Dual-Core, phân chia Core 0 (Network) và Core 1 (Sensor/IO), máy trạng thái nút nhấn GPIO 23.
- [UI:dashboard] Xây dựng bản mẫu giao diện người dùng hoàn chỉnh (public/prototype_ui.html) chuẩn UX/UI Pro Max chạy Offline và tối ưu bộ nhớ Flash cho ESP32.
- [Backend] Hoàn thiện bản thiết kế kiến trúc hệ thống SME-02, sơ đồ luồng dữ liệu, bảo mật License HMAC-SHA256 và cơ chế vòng đệm 90 ngày.
- [Backend] Khởi tạo mã nguồn mẫu chẩn đoán phần cứng ESP32 (Kiểm tra chip, Wi-Fi scan, telemetry heap, LED heartbeat và Serial CLI).
- [Chore] Cấu hình PlatformIO môi trường esp32dev cùng các script nạp flash_test.bat và monitor.bat tự động.
- [Backend] Tách `LICENSE_SALT_SECRET` và `LICENSE_SALT_VERSION` sang `.env` (nạp lúc biên dịch qua `load_env.py`), loại bỏ hoàn toàn Salt bí mật khỏi mã nguồn GitHub.
- [Backend] Nâng cấp OTA HTTPS hỗ trợ nạp đồng thời System (`U_SPIFFS` littlefs.bin) + Firmware (`U_FLASH`), kiểm tra dung lượng (`check_space`), nạp vòng 2 bước qua `esp32minimal` và quay về bản cũ (`rollback`).
- [UI:ota] Tích hợp danh sách chọn phiên bản từ `manifest.json` repo `sme-fimware-release`, hiển thị changelog 1-2 dòng, cảnh báo đổi Salt và nút kiểm tra dung lượng.
- [UI:license] Bổ sung hiển thị FW Version / Salt Version và nút "Quay Về Firmware Cũ Trước Khi Update" trên màn hình khóa License.
- [Chore] Tạo `release_firmware.bat` tự động biên dịch (FW + System + Minimal), nhập note mô tả thay đổi và đẩy bản binary sạch lên repo phát hành OTA; cập nhật `tag.ps1`/`release.ps1` xuất kèm `.bin`.
- [Fix] Chuẩn hóa 100% mã ASCII (`\uXXXX` runtime) cho `release_firmware.ps1`/`release.ps1`/`tag.ps1` chống mất BOM, khắc phục `NativeCommandError` khi chạy Git trong `release_firmware.ps1` và xử lý lỗi báo đỏ IDE (`load_env.py`, `.clangd`).
- [Chore] Bổ sung script build_ota.bat tự động biên dịch và gom file .bin (Firmware và Filesystem LittleFS) phục vụ nâng cấp từ xa qua OTA.
- [UI:dashboard] Đồng bộ hiển thị phiên bản Firmware FW v1.0.3 trên thanh tiêu đề và cập nhật động qua API Telemetry/Status.
- [Chore] Nâng cấp tag.ps1/release.ps1 tự động đồng bộ phiên bản Git vào include/app_config.h, data/index.html và public/prototype_ui.html.
- [Backend] Tích hợp endpoint API /api/v1/wifi/stop_ap hỗ trợ tắt SoftAP chủ động từ Web Client giúp điện thoại tự động chuyển về Wi-Fi Router.
- [Backend] Kích hoạt CORS (enableCORS) và hỗ trợ lưu cấu hình Wi-Fi cả dạng JSON lẫn URL-encoded form data tương thích PWA Smart Config.
- [Backend] Bổ sung bí danh API /api/v1/wifi/save và /api/v1/license/save đồng bộ giao thức nạp cấu hình ngoại tuyến.
- [Fix] Loại bỏ khối 2 nút "Khởi Động Lại / Khôi Phục Gốc" bị dán trùng lặp bên ngoài cổng License, trả lại thanh Header Full Web gọn gàng đúng thiết kế.
- [Fix] Đọc địa chỉ MAC trực tiếp từ eFuse phần cứng (esp_read_mac) khắc phục triệt để lỗi DHCP Hostname router bị trả về đuôi SME_000000000000.
- [Fix] Giữ nguyên kết nối STA khi quét Wi-Fi trong chế độ Client, loại bỏ hoàn toàn lỗi ngắt mạng và timeout khi quét Wi-Fi từ Full Web.
- [Fix] Xóa bỏ 100% dữ liệu mockup tĩnh trong bảng Hardware Tab 6 và thẻ License Tab 3, thay thế bằng dữ liệu phần cứng thời gian thực.
- [Fix] Xóa bỏ hoàn toàn chuỗi Wi-Fi mẫu NhaTro_Tang1 còn sót lại trong NVS default và bổ sung cập nhật m_config tức thì sau khi lưu API.
- [Fix] Khắc phục triệt để lỗi ERR_CONNECTION_TIMED_OUT khi quét Wi-Fi bằng cơ chế scan theo yêu cầu (on-demand), giải phóng trạng thái STA connecting và tối ưu thời gian quét 1.5s.
- [Fix] Khắc phục triệt để lỗi cú pháp JavaScript (newline trong alert) làm treo script trình duyệt; kích hoạt ẩn mặc định #screen-full-app và ưu tiên mở ngay AP gate trên 192.168.4.1.
- [Fix] Khắc phục triệt để lỗi phân tích cú pháp ký tự tiếng Việt trong tag.ps1/release.ps1 và chuyển tag.bat/release.bat sang chế độ nạp UTF-8 scriptblock.
- [Fix] Khắc phục lỗi Resolve-Path với public\version.json chưa tồn tại trong tag.ps1 và release.ps1 bằng GetFullPath.
- [Fix] Khắc phục lỗi cú pháp mã hóa tiếng Việt (ANSI vs UTF-8) trên Windows PowerShell cho tag.ps1 và release.ps1 bằng UTF-8 BOM.
- [Fix] Khắc phục lỗi WinError 183 trong PlatformIO builder gây crash khởi tạo metadata và bổ sung .vscode/tasks.json.
- [Fix] Bổ sung tự động dò tìm đường dẫn pio trong flash_test.bat và monitor.bat khi chưa add vào PATH.
- [Fix] Bổ sung thư viện esp_chip_info.h và chuẩn hóa format specifier tương thích ESP-IDF v5 (Arduino core 3.x).
- [Fix] Cấu hình c_cpp_properties.json, compile_commands.json và stdint.h khắc phục lỗi clang thiếu Arduino.h trên VSCode.

