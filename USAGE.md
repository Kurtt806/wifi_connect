# ESP32 Wi-Fi Connect Component

## Overview

This ESP-IDF component provides a complete Wi-Fi connection management solution for ESP32 devices. It includes:

- **WiFiManager**: Unified WiFi connection management
- **WiFi Station**: Client mode connectivity
- **WiFi Configuration AP**: Access Point mode with web interface
- **DNS Server**: Local DNS resolution
- **SSID Manager**: Multiple network management

## Features

- **Smart Connection**: Automatically connects to saved networks or starts configuration AP
- **Web Interface**: Modern, responsive web UI for Wi-Fi configuration
- **Multi-language**: Support for Vietnamese, English, and Chinese
- **Advanced Options**: OTA URL, TX power, BSSID settings
- **Responsive Design**: Optimized for desktop and mobile devices
- **Apple-inspired UI**: Clean, modern design with orange theme

## Usage

### Basic Usage

```cpp
#include "wifi_manager.h"

// Create configuration
WifiManagerConfig config;
config.ssid_prefix = "MyESP32";
config.language = "vi-VN";

// Get instance and initialize
auto& wifi = WifiManager::GetInstance();
wifi.Initialize(config);

// Set event callback
wifi.SetEventCallback([](WifiEvent event) {
    switch (event) {
        case WifiEvent::Connected:
            ESP_LOGI(TAG, "WiFi connected!");
            break;
        case WifiEvent::ConfigModeEnter:
            ESP_LOGI(TAG, "Configuration AP started - connect to http://192.168.4.1");
            break;
        case WifiEvent::Disconnected:
            ESP_LOGI(TAG, "WiFi disconnected");
            break;
    }
});

// Start WiFi
wifi.StartStation();
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
- **Saved Wi-Fi Panel**: View and manage stored networks (right panel)

## Dependencies

- ESP-IDF >= 5.3
- esp_timer
- esp_http_server
- esp_wifi
- nvs_flash
- json (cJSON)

## Installation

### As Component

Copy this directory to your project's `components/` folder:

```
your-project/
├── components/
│   └── khoa_wifi/
│       ├── CMakeLists.txt
│       ├── idf_component.yml
│       ├── wifi_manager.cc
│       ├── include/
│       └── assets/
├── main/
└── CMakeLists.txt
```

### As ESP Registry Component

Add to your `idf_component.yml`:

```yaml
dependencies:
  kurtt806/wifi_connect: "^3.1.0"
```

## API Reference

### WifiManager

#### Methods

- `static WifiManager& GetInstance()` - Get singleton instance
- `esp_err_t Initialize(const WifiManagerConfig& config)` - Initialize with config
- `esp_err_t StartStation()` - Start WiFi station mode
- `esp_err_t Stop()` - Stop WiFi
- `void SetEventCallback(std::function<void(WifiEvent)> callback)` - Set event callback

#### Configuration Methods

- `void SetOtaUrl(const std::string& url)` - Set OTA URL
- `void SetMaxTxPower(int power)` - Set max TX power (8-80)
- `void SetSleepMode(bool enable)` - Enable/disable sleep mode
- `void SetRememberBssid(bool remember)` - Remember BSSID setting

### Events

- `WifiEvent::Scanning` - Started scanning for networks
- `WifiEvent::Connecting` - Connecting to network
- `WifiEvent::Connected` - Successfully connected
- `WifiEvent::Disconnected` - Disconnected from network
- `WifiEvent::ConfigModeEnter` - Entered configuration AP mode
- `WifiEvent::ConfigModeExit` - Exited configuration AP mode

## Building

```bash
idf.py build
idf.py flash
idf.py monitor
```

## License

MIT License