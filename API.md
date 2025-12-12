# ESP32 Wi-Fi Configuration API

This document describes the REST API endpoints provided by the ESP32 Wi-Fi configuration web server.

## API Endpoints

### Wi-Fi Management

- `GET /` - Main configuration page
- `POST /submit` - Submit Wi-Fi credentials
  ```json
  {
    "ssid": "WiFi_Name",
    "password": "WiFi_Password"
  }
  ```
- `GET /scan` - Scan available Wi-Fi networks
- `GET /saved/list` - Get list of saved Wi-Fi networks
- `GET /saved/delete?index=X` - Delete saved Wi-Fi network by index
- `GET /saved/set_default?index=X` - Set default Wi-Fi network by index

### Advanced Configuration

- `GET /advanced/config` - Get current advanced configuration
- `POST /advanced/submit` - Save advanced configuration
  ```json
  {
    "ota_url": "http://example.com/firmware.bin",
    "max_tx_power": 80,
    "remember_bssid": true,
    "sleep_mode": false
  }
  ```

### GPIO Configuration

- `GET /pins/config` - Get current GPIO configuration
- `POST /pins/submit` - Save GPIO configuration
  ```json
  {
    "gpio_led": 2,
    "gpio_button": 0,
    "gpio_relay": 4,
    "driver_screen": "ssd1306",
    "screen_scl": 22,
    "screen_sda": 21,
    "screen_mosi": -1,
    "screen_sck": -1,
    "screen_cs": -1,
    "screen_dc": -1,
    "screen_rst": -1
  }
  ```
- `GET /pins/default` - Get default GPIO configuration

### System Control

- `GET /done.html` - Success page after configuration
- `GET /exit` - Exit configuration mode

## GPIO Pin Configuration

The GPIO configuration supports the following components:

### Basic GPIO
- **LED Pin**: GPIO pin for status LED (-1 = disabled)
- **Button Pin**: GPIO pin for configuration button (-1 = disabled)
- **Relay Pin**: GPIO pin for relay control (-1 = disabled)

### Screen Configuration
- **Driver Selection**: Choose screen driver (SSD1306, ST7735, ILI9341, SH1106, ST7789)
- **I2C Interface** (for SSD1306/SH1106):
  - SCL: I2C clock pin
  - SDA: I2C data pin
- **SPI Interface** (for ST7735/ILI9341/ST7789):
  - MOSI: SPI data pin
  - SCK: SPI clock pin
  - CS: Chip select pin
  - DC: Data/command pin
  - RST: Reset pin (optional)

## Usage Example

```cpp
#include "wifi_configuration_ap.h"

// Create configuration AP instance
WifiConfigurationAp config_ap;

// Set custom SSID prefix (optional)
config_ap.SetSsidPrefix("MyDevice-");

// Start configuration mode
config_ap.Start();

// The device will create a Wi-Fi hotspot and web server
// Users can connect and configure Wi-Fi via web interface at http://192.168.4.1

// Stop configuration mode
config_ap.Stop();
```

## Response Format

All API endpoints return JSON responses. Success responses include:
```json
{"success": true}
```

Error responses include HTTP status codes and error messages.</content>
<parameter name="filePath">d:\ESP-IDF\component\khoa_wifi\API.md