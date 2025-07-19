# Automated ESP Aquarium Controller

This project is an ESP32-based aquarium control system that automates and monitors various aquarium equipment including lights, wave makers, air pumps, and temperature control.

## Features

- **Web Interface**: Control your aquarium from any device on your network
- **Equipment Control**: Manage multiple relays for different equipment:
  - Wave Maker (Relay 1)
  - Light with color cycling (Relay 2)
  - Air Pump (Relay 3)
  - Heater with temperature control (Relay 4)
- **Scheduling**: Set detailed schedules with day-of-week support
- **Temporary Scheduling**: Create one-time schedules that automatically expire after execution
- **Temperature Monitoring**: Real-time temperature display with configurable thresholds
- **Automatic Heater Control**: Maintain water temperature within set parameters
- **Manual Override**: Physical switches for direct control
- **Logging System**: Store and display system events
- **Email Notifications**: Receive system status emails and alerts
- **Error Detection**: Visual indication of system errors
- **NTP Time Synchronization**: Accurate timekeeping for schedules

## Hardware Requirements

- ESP32 development board
- 4-channel relay module
- DS18B20 temperature sensor
- 2 switches for manual override
- Status LED
- Power supply
- Aquarium equipment to control (Wavemaker, Lights, Air Pump, Heater)

## Wiring

- Relay 1 (Wave Maker): GPIO16
- Relay 2 (Light): GPIO17
- Relay 3 (Air Pump): GPIO18
- Relay 4 (Heater): GPIO19
- Switch 1 (Override 1): GPIO23
- Switch 2 (Override 2): GPIO22
- Error LED: GPIO21
- Temperature Sensor: GPIO26

## Installation

1. Clone this repository
2. Open the project in Arduino IDE or PlatformIO
3. Install required libraries:
   - WiFi
   - WebServer
   - WebSocketsServer
   - ArduinoJson
   - LittleFS
   - ESP_Mail_Client
   - OneWire
   - DallasTemperature
   - TimeLib
4. Configure your WiFi credentials and email settings in the code
5. Upload the code to your ESP32
6. Access the web interface via the ESP32's IP address

## Configuration

Edit these parameters in the code to match your setup:

```cpp
const char* ssid = "Your_WiFi_SSID";
const char* password = "Your_WiFi_Password";
const char* emailSenderAccount = "your_email@gmail.com";
const char* emailSenderPassword = "your_app_password";
const char* emailRecipient = "recipient_email@example.com";
```

## Usage

### Web Interface

Access the web interface by navigating to the ESP32's IP address in a web browser. From here you can:
- Control equipment manually
- View and set temperature thresholds
- Create and manage schedules
- Set up temporary one-time schedules
- View system logs
- Clear error states

### Scheduling Options

#### Regular Schedules
- Create schedules that run on specified days of the week
- Set start and end times for each schedule
- Enable/disable schedules as needed

#### Temporary Schedules
- Create one-time schedules that automatically expire after execution
- Set either start time, end time, or both
- Each relay can have up to 2 temporary schedules at a time
- Perfect for temporary overrides without modifying regular schedules

### Manual Overrides

- Activate Switch 1 to override and enable Wave Maker and Air Pump (Relay 1 and 3)
- Activate Switch 2 to override and enable Light (Relay 2)

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.