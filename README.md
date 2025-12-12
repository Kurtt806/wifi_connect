# ESP-IDF WiFi Component

## Giới thiệu

Đây là một component ESP-IDF cung cấp các chức năng quản lý WiFi cho các thiết bị ESP32. Component này bao gồm các module để quản lý kết nối WiFi, cấu hình Access Point, và quản lý SSID.

## Cấu trúc dự án

- `wifi_manager.cc` / `wifi_manager.h`: Quản lý tổng thể WiFi
- `wifi_station.cc` / `wifi_station.h`: Chức năng Station mode
- `wifi_configuration_ap.cc` / `wifi_configuration_ap.h`: Cấu hình Access Point
- `ssid_manager.cc` / `ssid_manager.h`: Quản lý SSID
- `dns_server.cc` / `dns_server.h`: Máy chủ DNS
- `assets/`: Các file HTML cho giao diện cấu hình WiFi

## Cách sử dụng

1. Thêm component này vào dự án ESP-IDF của bạn.
2. Bao gồm các header file cần thiết trong code của bạn.
3. Sử dụng các API được cung cấp để quản lý WiFi.

## Ví dụ sử dụng thực tế

Dưới đây là ví dụ cơ bản về cách sử dụng WifiManager để kết nối WiFi:

```cpp
#include "wifi_manager.h"

// Lấy instance của WifiManager
auto& wifi = WifiManager::GetInstance();

// Tạo event group để chờ sự kiện
EventGroupHandle_t events = xEventGroupCreate();

// Thiết lập callback cho các sự kiện WiFi
wifi.SetEventCallback([events](WifiEvent e) {
    if (e == WifiEvent::Connected) {
        xEventGroupSetBits(events, BIT0);  // Đã kết nối
    }
    if (e == WifiEvent::ConfigModeExit) {
        xEventGroupSetBits(events, BIT1);  // Thoát chế độ cấu hình
    }
});

// Khởi tạo với cấu hình mặc định
WifiManagerConfig config;
wifi.Initialize(config);

// Bắt đầu chế độ Station (kết nối WiFi)
wifi.StartStation();

// Chờ sự kiện kết nối hoặc thoát chế độ cấu hình
xEventGroupWaitBits(events, BIT0 | BIT1, pdTRUE, pdFALSE, portMAX_DELAY);

// Kiểm tra trạng thái kết nối
if (wifi.IsConnected()) {
    printf("Đã kết nối WiFi: %s\n", wifi.GetSsid().c_str());
    printf("Địa chỉ IP: %s\n", wifi.GetIpAddress().c_str());
}
```

Để khởi động chế độ Access Point cho cấu hình:

```cpp
// Bắt đầu chế độ cấu hình AP
wifi.StartConfigAp();

// Lấy URL của trang web cấu hình
std::string webUrl = wifi.GetApWebUrl();
printf("Truy cập %s để cấu hình WiFi\n", webUrl.c_str());
```

Dựa trên: https://github.com/78/esp-wifi-connect

## Yêu cầu

- ESP-IDF framework
- ESP32 board

## Cài đặt

Sao chép thư mục này vào thư mục `components` của dự án ESP-IDF của bạn.
