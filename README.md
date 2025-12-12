# 🌐 ESP32 Wi-Fi Connect

<div align="center">

![ESP32 Wi-Fi Connect](https://img.shields.io/badge/ESP32-WiFi-blue?style=for-the-badge&logo=espressif)
![C++](https://img.shields.io/badge/C%2B%2B-17-orange?style=for-the-badge&logo=c%2B%2B)
![ESP-IDF](https://img.shields.io/badge/ESP--IDF-5.5.1-green?style=for-the-badge&logo=espressif)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

**🚀 Cấu hình Wi-Fi thông minh cho thiết bị ESP32**

[📖 Tài liệu](#-tài-liệu) • [🔧 Cài đặt](#-cài-đặt) • [📡 API](#-api) • [🎯 Cách sử dụng](#-cách-sử-dụng) • [🤝 Đóng góp](#-đóng-góp)

</div>

---

## ✨ Tính năng

<div align="center">

### 🌟 Tính năng chính
| Tính năng | Mô tả |
|-----------|--------|
| 🔄 **Tự động kết nối** | Tự động kết nối với mạng Wi-Fi đã lưu |
| 📱 **Giao diện web** | Giao diện web đẹp cho việc cấu hình Wi-Fi |
| 🌍 **Đa ngôn ngữ** | Hỗ trợ tiếng Việt, Anh, Trung |
| 📶 **Chế độ AP** | Tạo hotspot khi không thể kết nối |
| ⚙️ **Cấu hình nâng cao** | Cập nhật OTA, cài đặt nguồn, chế độ ngủ |
| 🔌 **Cấu hình GPIO** | Cấu hình chân LED, nút nhấn, relay và màn hình |
| 🎨 **Giao diện hiện đại** | Thiết kế theo phong cách Apple với chủ đề sáng/tối |

### 🎯 Cấu hình thông minh
- **Captive Portal**: Tự động chuyển hướng đến trang cấu hình
- **Nhiều SSID**: Lưu trữ lên đến 10 mạng Wi-Fi
- **Quản lý ưu tiên**: Đặt mạng ưu tiên
- **Hỗ trợ 5G**: Hỗ trợ Wi-Fi 5G cho ESP32C5
- **SmartConfig**: Hỗ trợ ESPTouch v2

</div>

---

## 🎬 Demo

<div align="center">

### 🌐 Giao diện cấu hình Wi-Fi
<img src="assets/ap_v3.png" width="300" alt="Giao diện cấu hình Wi-Fi" style="border-radius: 10px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">

### ⚙️ Tùy chọn nâng cao
<img src="assets/ap_v3_advanced.png" width="300" alt="Cấu hình nâng cao" style="border-radius: 10px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);">

### 🎮 Cấu hình GPIO
*Giao diện modal hiện đại để cấu hình chân với lựa chọn driver*

</div>

---

## 📦 Cài đặt

### Thành phần ESP-IDF
```bash
# Thêm vào dự án ESP-IDF của bạn
cd components/
git clone https://github.com/your-repo/esp32-wifi-connect.git
```

### Phụ thuộc
- ESP-IDF v5.5.1+
- Thư viện cJSON
- Bộ nhớ flash NVS

---

## 🚀 Bắt đầu nhanh

```cpp
#include <wifi_manager.h>
#include <ssid_manager.h>

// Khởi tạo ESP-IDF
ESP_ERROR_CHECK(esp_event_loop_create_default());
ESP_ERROR_CHECK(nvs_flash_init());

// Lấy instance WiFi Manager
auto& wifi_manager = WifiManager::GetInstance();

// Cấu hình và khởi tạo
WifiManagerConfig config;
config.ssid_prefix = "MyDevice-";
config.language = "vi-VN";
wifi_manager.Initialize(config);

// Đặt callback sự kiện
wifi_manager.SetEventCallback([](WifiEvent event) {
    switch (event) {
        case WifiEvent::Connected:
            printf("🎉 Đã kết nối Wi-Fi!\n");
            break;
        case WifiEvent::ConfigModeEnter:
            printf("🔧 Vào chế độ cấu hình: http://192.168.4.1\n");
            break;
        default:
            break;
    }
});

// Bắt đầu quản lý Wi-Fi
if (SsidManager::GetInstance().GetSsidList().empty()) {
    wifi_manager.StartConfigAp();  // Bắt đầu chế độ cấu hình
} else {
    wifi_manager.StartStation();   // Kết nối với mạng đã lưu
}
```

---

## 📡 API

### 🌐 Điểm cuối Web

| Phương thức | Điểm cuối | Mô tả |
|-------------|-----------|--------|
| `GET` | `/` | Trang cấu hình chính |
| `POST` | `/submit` | Gửi thông tin đăng nhập Wi-Fi |
| `GET` | `/scan` | Quét mạng khả dụng |
| `GET` | `/saved/list` | Lấy mạng đã lưu |
| `GET` | `/advanced/config` | Lấy cài đặt nâng cao |
| `POST` | `/advanced/submit` | Lưu cài đặt nâng cao |
| `GET` | `/pins/config` | Lấy cấu hình GPIO |
| `POST` | `/pins/submit` | Lưu cấu hình GPIO |
| `GET` | `/pins/default` | Lấy cấu hình GPIO mặc định |

### 🔧 Cấu hình GPIO

```json
{
  "gpio_led": 2,
  "gpio_button": 0,
  "gpio_relay": 4,
  "driver_screen": "ssd1306",
  "screen_scl": 22,
  "screen_sda": 21
}
```

### 🎨 Driver màn hình được hỗ trợ
- **SSD1306** (128x64 OLED) - I2C
- **SH1106** (128x64 OLED) - I2C
- **ST7735** (128x160 TFT) - SPI
- **ILI9341** (320x240 TFT) - SPI
- **ST7789** (240x240 TFT) - SPI

---

## 🎯 Cách sử dụng nâng cao

### Cấu hình tùy chỉnh
```cpp
WifiManagerConfig config;
config.ssid_prefix = "SmartDevice-";
config.language = "zh-CN";
config.max_retry_count = 5;
wifi_manager.Initialize(config);
```

### Quản lý chân GPIO
```cpp
// Truy cập cấu hình GPIO
auto& config_ap = wifi_manager.GetConfigAp();
// Cài đặt GPIO được tải tự động từ NVS
```

### Xử lý sự kiện
```cpp
wifi_manager.SetEventCallback([](WifiEvent event) {
    switch (event) {
        case WifiEvent::Scanning:
            // Hiển thị animation quét
            break;
        case WifiEvent::Connected:
            // Cập nhật trạng thái kết nối
            break;
        case WifiEvent::ConfigModeEnter:
            // Hiển thị hướng dẫn cấu hình
            break;
    }
});
```

---

## 🔄 Nhật ký thay đổi

### v3.0.0 🎉
- ✨ Thêm cấu hình GPIO với giao diện modal
- 🎨 Thiết kế UI hiện đại theo phong cách Apple
- 🌙 Hỗ trợ chủ đề sáng/tối
- 🔧 Cải thiện xử lý lỗi và quản lý trạng thái
- 📱 Cải thiện khả năng đáp ứng trên thiết bị di động

### v2.6.0 📶
- 🌐 Thêm hỗ trợ Wi-Fi 5G cho ESP32C5

### v2.4.0 🌍
- 💬 Thêm ngôn ngữ tiếng Việt, Trung phồn thể
- ⚙️ Tab cấu hình nâng cao
- 🔌 Tối ưu hóa kết nối

### v2.2.0 🛠️
- 📱 Hỗ trợ ESP32 SmartConfig (ESPTouch v2)

---

## 🤝 Đóng góp

<div align="center">

**Chúng tôi chào đón mọi đóng góp!** 🎉

1. Fork repository
2. Tạo nhánh tính năng của bạn (`git checkout -b feature/tinh-nang-tuyet-voi`)
3. Commit thay đổi của bạn (`git commit -m 'Thêm tính năng tuyệt vời'`)
4. Push lên nhánh (`git push origin feature/tinh-nang-tuyet-voi`)
5. Mở Pull Request

### Thiết lập phát triển
```bash
# Clone và thiết lập ESP-IDF
git clone https://github.com/espressif/esp-idf.git
cd esp-idf
./install.sh
. ./export.sh

# Build thành phần
idf.py build
```

</div>

---

## 📚 Tài liệu

- 📖 [Hướng dẫn lập trình ESP-IDF](https://docs.espressif.com/projects/esp-idf/)
- 🔧 [Tài liệu kỹ thuật ESP32](https://www.espressif.com/sites/default/files/documentation/esp32_technical_reference_manual_en.pdf)
- 🌐 [API Wi-Fi](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/network/esp_wifi.html)

---

## 🎨 Tính năng giao diện

### Các yếu tố thiết kế hiện đại
- 🍎 Font SF Pro theo phong cách Apple
- 🎭 Animation và chuyển tiếp mượt mà
- 🌈 Bảng màu cam làm điểm nhấn
- 📱 Thiết kế đáp ứng cho mọi thiết bị
- 🎪 Modal dialogs cho cấu hình phức tạp

### Khả năng truy cập
- ♿ Hỗ trợ trình đọc màn hình
- ⌨️ Điều hướng bằng bàn phím
- 🎯 Chế độ tương phản cao
- 🌍 Hỗ trợ đa ngôn ngữ

---

## ⚠️ Lưu ý quan trọng

- **Bộ nhớ NVS**: Thông tin đăng nhập Wi-Fi được lưu trong namespace "wifi"
- **Cấu hình GPIO**: Cài đặt chân được lưu trong namespace "gpio"
- **Bộ nhớ**: Thành phần sử dụng ~50KB RAM trong chế độ cấu hình
- **Bảo mật**: Giao diện web chỉ truy cập được trên mạng cục bộ

---

## 📄 Giấy phép

<div align="center">

**Giấy phép MIT** - Tự do sử dụng trong dự án của bạn! 🚀

Bản quyền © 2025 ESP32 Wi-Fi Connect

</div>

---

## 🙏 Lời cảm ơn

<div align="center">

**Dựa trên công việc gốc từ:**
### [78/xiaozhi-esp32](https://github.com/78/xiaozhi-esp32) ⭐

*Cảm ơn đặc biệt cộng đồng ESP32 và Espressif vì công việc tuyệt vời của họ!*

---

**Được tạo với ❤️ cho cộng đồng ESP32**

[⬆️ Về đầu trang](#-esp32-wi-fi-connect)

</div>
