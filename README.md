# Smart_Embed - ESP32-C3 IoT Distance Monitoring System

[![ESP32-C3](https://img.shields.io/badge/ESP32--C3-RISC--V-blue)](https://www.espressif.com/en/products/socs/esp32-c3)
[![FreeRTOS](https://img.shields.io/badge/FreeRTOS-Real--time-green)](https://www.freertos.org/)
[![WiFi](https://img.shields.io/badge/WiFi-Station--Mode-orange)](https://docs.espressif.com/projects/esp-idf/en/latest/esp32c3/api-guides/wifi.html)
[![License](https://img.shields.io/badge/License-CC0--1.0-lightgrey)](https://creativecommons.org/publicdomain/zero/1.0/)

## 📋 Tổng quan

**Smart_Embed** là hệ thống giám sát khoảng cách thông minh sử dụng ESP32-C3, tích hợp cảm biến siêu âm HC-SR04, màn hình OLED, và giao diện web để theo dõi và điều khiển thiết bị từ xa. Hệ thống được thiết kế với kiến trúc FreeRTOS đa tác vụ, đảm bảo hiệu suất cao và phản hồi real-time.

### ✨ Tính năng chính

- 🔍 **Giám sát khoảng cách real-time** với cảm biến siêu âm HC-SR04
- 💡 **Cảnh báo LED tự động** khi phát hiện vật thể gần
- 💾 **Lưu trữ dữ liệu** trên thẻ SD với định dạng CSV

## 🏗️ Kiến trúc hệ thống

### Cấu trúc dự án

```
Smart_Embed/
├── main/                          # Source code chính
│   ├── Smart_Embed.c             # File chính với FreeRTOS tasks
│   ├── index.html                # Web interface (embedded)
│   ├── CMakeLists.txt            # Build configuration
│   └── Kconfig.projbuild         # Configuration options
├── components/                    # Custom components
│   ├── ultrasonic_sensor/        # Driver cảm biến siêu âm
│   ├── oled_driver/              # Driver màn hình OLED
│   ├── sd_card_spi/              # Driver thẻ SD
├── build/                        # Build output
├── sdkconfig                     # ESP-IDF configuration
└── README.md                     # Documentation này
```

### FreeRTOS Tasks

| Task | Priority | Stack | Period | Chức năng |
|------|----------|-------|--------|-----------|
| `sensor_task` | 3 | 4KB | 500ms | Đọc cảm biến siêu âm |
| `led_task` | 2 | 2KB | 100ms | Điều khiển LED cảnh báo |
| `display_task` | 2 | 4KB | 200ms | Cập nhật màn hình OLED |
| `sdcard_task` | 2 | 4KB | 100ms | Lưu dữ liệu vào thẻ SD |
| `app_main` | 1 | 8KB | 10s | Monitor hệ thống |

## 🔧 Kết nối phần cứng

### ESP32-C3 Pin Mapping

| Chức năng | GPIO | Mô tả |
|-----------|------|-------|
| **OLED Display (SSD1306)** |
| SDA | 3 | I2C Data |
| SCL | 4 | I2C Clock |
| **Ultrasonic Sensor (HC-SR04)** |
| TRIG | 6 | Trigger Signal |
| ECHO | 7 | Echo Signal |
| **LED Warning** |
| LED | 1 | Warning LED (với điện trở 220Ω) |
| **SD Card (SPI)** |
| MISO | 19 | SPI Data In |
| MOSI | 23 | SPI Data Out |
| CLK | 18 | SPI Clock |
| CS | 5 | SPI Chip Select |

### Sơ đồ kết nối

```
ESP32-C3                    OLED Display
┌─────────┐                 ┌─────────┐
│ GPIO 3  │ ─────────────── │   SDA   │
│ GPIO 4  │ ─────────────── │   SCL   │
│   3.3V  │ ─────────────── │   VCC   │
│   GND   │ ─────────────── │   GND   │
└─────────┘                 └─────────┘

ESP32-C3                  HC-SR04 Sensor
┌─────────┐                 ┌─────────┐
│ GPIO 6  │ ─────────────── │  TRIG   │
│ GPIO 7  │ ─────────────── │  ECHO   │
│   5V    │ ─────────────── │   VCC   │
│   GND   │ ─────────────── │   GND   │
└─────────┘                 └─────────┘

ESP32-C3                    LED Warning
┌─────────┐                 ┌─────────┐
│ GPIO 1  │ ────[220Ω]───── │ Anode   │
│   GND   │ ─────────────── │ Cathode │
└─────────┘                 └─────────┘

ESP32-C3                      SD_CARD
┌─────────┐                 ┌─────────┐
│ GPIO 5  │ ─────────────── │   CS    │
│ GPIO 18 │ ─────────────── │   SCL   │
│ GPIO 23 │ ─────────────── │   MOSI  │
│ GPIO 19 │ ─────────────── │   MISO  │
└─────────┘                 └─────────┘
```

## Xem Logs thời gian thực



```bash
# Xem logs real-time
idf.py monitor

# Lọc logs theo component
idf.py monitor --print-filter="smart_embed:*,ultrasonic:*,oled_driver:*"
```

### Performance Metrics

Hệ thống tự động log các thông tin sau:

```
I (5000) smart_embed: Distance: 25.3 cm
I (5200) smart_embed: LED ON - Distance: 8.5 cm < 10.0 cm
I (5400) smart_embed: System running - Distance: 25.3 cm, Valid: Yes, LED: OFF
```















