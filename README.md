# Smart Board Control System

An ESP32-based intelligent relay control system with web interface, scheduling capabilities, and real-time monitoring. This versatile automation system can control up to 4 relays independently through a responsive web dashboard, physical switches, or automated schedules.

## Features

### Core Functionality
- **4-Channel Relay Control**: Independent control of 4 separate relays for appliances or devices
- **Responsive Web Interface**: Modern, mobile-friendly dashboard accessible from any device on your network
- **Real-Time Updates**: WebSocket-based live relay status updates without page refresh
- **Physical Switch Override**: 4 manual switches with debouncing for direct relay control
- **LED Status Indicators**: Individual LED for each relay showing current state

### Advanced Scheduling
- **Recurring Schedules**: Set up schedules with:
  - Specific days of the week (Monday-Sunday)
  - Custom on/off times
  - Enable/disable functionality
  - Up to 10 schedules stored in EEPROM
- **Temporary Schedules**: One-time schedules that auto-expire after execution
  - Flexible on-time, off-time, or both
  - Perfect for temporary overrides without affecting recurring schedules
  - Multiple temporary schedules per relay

### System Features
- **NTP Time Synchronization**: Automatic time sync from NTP servers (IST timezone configured)
- **Persistent Storage**: Schedules saved to EEPROM, survive power cycles
- **Event Logging**: Comprehensive logging system with timestamps stored in LittleFS
  - System events
  - Relay state changes
  - WiFi connection status
  - Error notifications
- **Error Detection & Recovery**: 
  - Visual LED blinking on system errors
  - Automatic error detection for WiFi and time sync failures
  - Manual error clearing via web interface
- **Watchdog Timer**: Automatic system restart on hang/freeze
- **Dual-Core Processing**: Optimized task distribution across ESP32 cores
  - Core 0: Network and web server operations
  - Core 1: Relay control and scheduling logic

### Security
- **Basic Authentication**: Password protection for web interface
- **IP Whitelist**: Configure allowed IP addresses for access without password
- **Secure Access Control**: Two-layer security approach

## Hardware Requirements

### Components
- ESP32 development board
- 4-channel relay module (active LOW trigger)
- 4 push buttons/switches (for manual control)
- 4 LEDs with appropriate resistors (status indicators)
- Power supply (5V recommended)
- Breadboard and jumper wires (for prototyping)

### GPIO Pin Assignments

| Component | GPIO Pin | Description |
|-----------|----------|-------------|
| Relay 1 | GPIO 13 | First relay control (Socket 1) |
| Relay 2 | GPIO 12 | Second relay control (Socket 2) |
| Relay 3 | GPIO 14 | Third relay control (Socket 3) |
| Relay 4 | GPIO 27 | Fourth relay control (Socket 4) |
| Switch 1 | GPIO 26 | Manual control button 1 |
| Switch 2 | GPIO 25 | Manual control button 2 |
| Switch 3 | GPIO 33 | Manual control button 3 |
| Switch 4 | GPIO 32 | Manual control button 4 |
| LED 1 | GPIO 18 | Status indicator for Relay 1 |
| LED 2 | GPIO 19 | Status indicator for Relay 2 |
| LED 3 | GPIO 22 | Status indicator for Relay 3 |
| LED 4 | GPIO 23 | Status indicator for Relay 4 |

## Installation

### Prerequisites
1. Arduino IDE 1.8.x or later, or PlatformIO
2. ESP32 board support installed

### Required Libraries
Install the following libraries:
- **WiFi** (Built-in with ESP32)
- **WebServer** (Built-in with ESP32)
- **WebSocketsServer** by Markus Sattler
- **ArduinoJson** by Benoit Blanchon (version 6.x)
- **EEPROM** (Built-in)
- **LittleFS** (Built-in with ESP32)
- **TimeLib** by Paul Stoffregen
- **Ticker** (Built-in with ESP32)

### Setup Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/Armaan4477/Smart-Board-Control.git
   cd Smart-Board-Control
   ```

2. **Configure WiFi Credentials**
   
   Open `automation/automation.ino` and update:
   ```cpp
   const char* ssid = "Your_WiFi_SSID";
   const char* password = "Your_WiFi_Password";
   ```

3. **Configure Authentication** (Optional)
   
   Update login credentials:
   ```cpp
   const char* authUsername = "admin";
   const char* authPassword = "12345678";
   ```

4. **Configure IP Whitelist** (Optional)
   
   Add your device IP addresses to bypass authentication:
   ```cpp
   const std::vector<String> allowedIPs = {
     "192.168.1.100",  // Your PC
     "192.168.1.101",  // Your Phone
   };
   ```

5. **Upload to ESP32**
   - Select your ESP32 board from Tools > Board
   - Select the correct COM port
   - Click Upload

6. **Monitor Serial Output**
   - Open Serial Monitor at 115200 baud
   - Note the assigned IP address

## Usage

### Web Interface Access

1. Connect to the same WiFi network as the ESP32
2. Open a web browser and navigate to: `http://[ESP32_IP_ADDRESS]`
3. Login with configured credentials (if not on whitelist)

### Main Dashboard Features

#### Relay Control
- **Individual Control**: Toggle each relay ON/OFF with dedicated buttons
- **Real-Time Status**: See current state of all relays
- **Visual Feedback**: Color-coded buttons (green=ON, red=OFF)

#### Time Display
- Current time in HH:MM:SS format
- Current day of the week
- Current date (DD/MM/YYYY)
- Auto-updates every second

#### Navigation
- **Main Schedules**: View and manage recurring schedules
- **Temp Schedules**: View and manage temporary one-time schedules
- **Logs**: View system event logs
- **Clear Errors**: Reset error states

### Schedule Management

#### Creating Regular Schedules
1. Navigate to "Main Schedules" page
2. Click "Add New Schedule"
3. Configure:
   - Relay number (1-4)
   - On time (HH:MM)
   - Off time (HH:MM)
   - Days of week (select multiple)
   - Enabled status
4. Click "Add Schedule"
5. Schedule automatically saves to EEPROM

#### Creating Temporary Schedules
1. Navigate to "Temp Schedules" page
2. Click "Add Temporary Schedule"
3. Configure:
   - Relay number (1-4)
   - On time (optional)
   - Off time (optional)
4. Click "Add"
5. Schedule executes once and auto-deletes

### Physical Switch Control

- Press any switch to toggle corresponding relay
- Debounce delay: 800ms to prevent accidental triggers
- Works independently of web interface
- Override schedules when manually activated

### Viewing Logs

1. Navigate to "Logs" page
2. View chronological list of events:
   - System startup
   - WiFi connection status
   - Relay state changes
   - Schedule executions
   - Error conditions
3. Maximum 18 log entries stored

### Error Management

**Error Indicators:**
- All LEDs blink simultaneously (1 second interval)
- Error status visible in web interface

**Clearing Errors:**
1. Click "Clear Errors" button on main page
2. Or navigate to `/error/clear`
3. LED blinking stops
4. System resumes normal operation

## API Endpoints

### Web Pages
- `GET /` - Main control dashboard
- `GET /mainSchedules` - Regular schedules management page
- `GET /tempschedules` - Temporary schedules management page
- `GET /logs` - System logs viewer

### Relay Control
- `GET/POST /relay/1` - Control Relay 1
- `GET/POST /relay/2` - Control Relay 2
- `GET/POST /relay/3` - Control Relay 3
- `GET/POST /relay/4` - Control Relay 4
- `GET /relay/status` - Get all relay states (JSON)

### Schedule Management
- `GET /schedules` - Get all regular schedules (JSON)
- `POST /schedule/add` - Add new regular schedule
- `POST /schedule/update` - Update existing schedule
- `DELETE /schedule/delete` - Delete schedule
- `GET /temp-schedules` - Get temporary schedules (JSON)
- `POST /temp-schedule/add` - Add temporary schedule
- `DELETE /temp-schedule/delete` - Delete temporary schedule

### System
- `GET /time` - Get current system time (JSON)
- `GET /logs/data` - Get system logs (JSON)
- `GET /error/status` - Get error status (JSON)
- `POST /error/clear` - Clear error state
- `GET /favicon.png` - Dashboard favicon

## Configuration Options

### Time Zone Configuration
Default: IST (Indian Standard Time, UTC+5:30)
```cpp
const long gmtOffset_sec = 19800;  // Adjust for your timezone
const int daylightOffset_sec = 0;  // Daylight saving offset
```

### NTP Server
```cpp
const char* ntpServer = "pool.ntp.org";  // Change if needed
```

### Watchdog Timeout
```cpp
const unsigned long watchdogTimeout = 10000;  // 10 seconds
```

### Debounce Delay
```cpp
const unsigned long DEBOUNCE_DELAY = 800;  // 800ms
```

### Maximum Schedules
```cpp
const int MAX_SCHEDULES = 10;  // Adjust as needed
```

## Troubleshooting

### WiFi Connection Issues
- Verify SSID and password are correct
- Check if ESP32 is within WiFi range
- Look for "WiFi connection failed" in serial monitor
- System will indicate error with blinking LEDs

### Time Sync Failures
- Ensure internet connectivity
- Check NTP server accessibility
- Verify timezone settings
- Error will be logged and LEDs will blink

### Relay Not Responding
- Check wiring connections
- Verify relay module is powered
- Test with physical switches first
- Check relay module trigger type (active HIGH/LOW)

### Schedule Not Executing
- Verify time is synced correctly
- Check schedule is enabled
- Confirm correct days of week selected
- Ensure on/off times are valid

### Web Interface Not Loading
- Confirm ESP32 IP address
- Check if device is on same network
- Try accessing from different browser
- Clear browser cache

## Technical Specifications

- **Microcontroller**: ESP32 (dual-core, 240MHz)
- **Memory**: 
  - EEPROM: 512 bytes (schedule storage)
  - LittleFS: Used for log storage
- **Network**: WiFi 802.11 b/g/n
- **Web Server**: Port 80 (HTTP)
- **WebSocket**: Port 81
- **Relay Control**: Active LOW trigger
- **Input Voltage**: 5V DC (via USB) or 3.3V (depending on board)
- **Maximum Current per Relay**: Depends on relay module (typically 10A @ 250VAC)

## Development

### Project Structure
```
Smart-Board-Control/
├── automation/
│   └── automation.ino    # Main firmware file
├── README.md            # This file
└── LICENSE             # MIT License
```

### Key Functions
- `setup()` - Initialize hardware, WiFi, and web server
- `mainLoop()` - Core 1 task (relay control, scheduling)
- `secondaryLoop()` - Core 0 task (network operations)
- `checkSchedules()` - Evaluate and execute regular schedules
- `checkTemporarySchedules()` - Evaluate temporary schedules
- `checkPushButton[1-4]()` - Handle physical switch inputs
- `handleRelay[1-4]()` - Web interface relay control handlers

## Safety Notes

⚠️ **Important Safety Information**

1. **Electrical Safety**: Relays can control high voltage AC devices. Ensure proper insulation and wiring.
2. **Load Ratings**: Do not exceed relay module specifications
3. **Cooling**: Ensure adequate ventilation for ESP32 and relay module
4. **Isolation**: Use optocoupler-based relay modules for electrical isolation
5. **Enclosure**: House the system in a proper enclosure when controlling AC loads
6. **Testing**: Test thoroughly with low-voltage devices before deploying with AC loads


## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.