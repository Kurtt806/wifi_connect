# ESP32 Wi-Fi Connect Component

[![Component Registry](https://components.espressif.com/components/kurtt806/wifi_connect/badge.svg)](https://components.espressif.com/components/kurtt806/wifi_connect)

A comprehensive ESP32 Wi-Fi configuration component with modern web interface for IoT devices.

## Features

- **Smart Wi-Fi Management**: Automatically connects to saved networks or starts configuration AP mode
- **Modern Web UI**: Apple-inspired responsive design with orange theme
- **Multi-language Support**: Vietnamese, English, and Chinese
- **Advanced Configuration**: OTA updates, TX power control, sleep mode
- **Thread-safe**: Mutex-protected operations
- **ESP Registry Ready**: Easy installation via IDF Component Manager

## Installation

### Via ESP-IDF Component Manager

```bash
idf.py add-dependency "kurtt806/wifi_connect>=3.1.0"
```

### Manual Installation

Copy this component to your project's `components` directory:

```bash
git clone https://github.com/Kurtt806/wifi_connect.git components/khoa_wifi
```

## Usage

### Basic Example

```cpp
#include "wifi_manager.h"

extern "C" void app_main(void)
{
    // Initialize NVS
    ESP_ERROR_CHECK(nvs_flash_init());

    // Configure WiFi Manager
    WifiManagerConfig config;
    config.ssid_prefix = "MyESP32";
    config.language = "vi-VN";

    // Get instance and setup
    auto& wifi = WifiManager::GetInstance();
    wifi.SetEventCallback([](WifiEvent event) {
        switch (event) {
            case WifiEvent::Connected:
                ESP_LOGI("WIFI", "Connected to WiFi!");
                break;
            case WifiEvent::ConfigModeEnter:
                ESP_LOGI("WIFI", "AP mode: http://192.168.4.1");
                break;
        }
    });

    // Start WiFi
    ESP_ERROR_CHECK(wifi.Initialize(config));
    ESP_ERROR_CHECK(wifi.StartStation());
}
```

### Advanced Configuration

```cpp
// Configure advanced settings
wifi.SetOtaUrl("http://example.com/firmware.bin");
wifi.SetMaxTxPower(80); // 20 dBm
wifi.SetSleepMode(true);
wifi.SetRememberBssid(true);
```

## Web Interface

When in configuration mode, access the web interface at `http://192.168.4.1`:

- **New Wi-Fi Tab**: Scan and connect to available networks
- **Advanced Tab**: Configure OTA, TX power, and other settings
- **Saved Wi-Fi Panel**: View and manage stored networks (fixed right panel)

## API Reference

### WifiManager

#### Core Methods
- `static WifiManager& GetInstance()` - Get singleton instance
- `esp_err_t Initialize(const WifiManagerConfig& config)` - Initialize with config
- `esp_err_t StartStation()` - Start WiFi station mode
- `esp_err_t Stop()` - Stop WiFi operations

#### Configuration Methods
- `void SetOtaUrl(const std::string& url)` - Set OTA update URL
- `void SetMaxTxPower(int power)` - Set max TX power (8-80)
- `void SetSleepMode(bool enable)` - Enable/disable sleep mode
- `void SetRememberBssid(bool remember)` - Remember BSSID setting

#### Events
- `WifiEvent::Scanning` - Started scanning for networks
- `WifiEvent::Connecting` - Connecting to network
- `WifiEvent::Connected` - Successfully connected
- `WifiEvent::Disconnected` - Disconnected from network
- `WifiEvent::ConfigModeEnter` - Entered configuration AP mode
- `WifiEvent::ConfigModeExit` - Exited configuration AP mode

## Dependencies

- ESP-IDF >= 5.3
- esp_timer
- esp_http_server
- esp_wifi
- nvs_flash
- json (cJSON)

## Building

```bash
idf.py build
idf.py flash
idf.py monitor
```

## Screenshots

### Wi-Fi Configuration Interface
![Wi-Fi Configuration](assets/ap_v3.png)

### Advanced Options
![Advanced Configuration](assets/ap_v3_advanced.png)

## License

MIT License

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.
