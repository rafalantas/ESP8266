# ESP8266 Weather Station

ESP8266 weather station with:
- BME280 for temperature, humidity, and pressure
- SDS011 for PM2.5 and PM10
- Local web panel over HTTP
- mDNS access in the local network
- APRS telemetry and weather packets
- ThingSpeak upload
- Windy Stations API v2 upload

## Features

- Local status page with live JSON and readable cards
- Automatic Wi-Fi reconnect
- NTP time sync for APRS timestamps and Windy uploads
- SDS011 wake, warmup, sample, and sleep cycle
- Basic sensor sanity checks for temperature, humidity, and pressure
- Separation of private configuration into `secrets.h`

## Project files

```text
esp8266_wx.ino
secrets.h
secrets.example.h
.gitignore
README.md
```

## Hardware

- ESP8266 board
- BME280
- SDS011

## Pins

| Signal | Pin |
|---|---|
| I2C SDA | D2 |
| I2C SCL | D1 |
| SDS011 RX | D6 |
| SDS011 TX | D7 |

## Required libraries

Install these libraries in Arduino IDE or PlatformIO:

- ESP8266 core for Arduino
- Adafruit BME280 Library
- Adafruit Unified Sensor

The sketch also uses built-in ESP8266 libraries such as WiFi, WebServer, mDNS, BearSSL, Wire, and SoftwareSerial.

## Secrets and private config

Private values are stored in `secrets.h`, while a safe template for GitHub is stored in `secrets.example.h`.

Create your local `secrets.h` based on the example and never commit it to Git. Keep `.gitignore` with:

```gitignore
secrets.h
```

## Windy Stations API v2

Windy Stations now uses the Stations API host `https://stations.windy.com/` and authenticates weather uploads with a **station password**.[1][2]

The current API reference says there are two auth types:
- **Station password** for uploading and reading station data.[1]
- **API key** for station management and open-data access.[1][2]

The legacy API is deprecated and remains operational only until the end of 2026, so new firmware should use the v2 API now.[1]

### Where to get Station ID and Station Password

1. Sign in at [stations.windy.com](https://stations.windy.com/).[2]
2. Open **My Stations** and create a station if you do not have one yet.[2]
3. Open the station detail page.[2]
4. Copy:
   - **Station ID** from the station details / connection page.
   - **Station password** from the same station details page in **My Stations**.[2][1]

Windy documentation states that the station password is auto-generated per station and visible on the station detail page in **My Stations**.[1]

### Important Windy note

After creating a station, Windy says there may be a delay of up to 15 minutes before the station appears on the map, and any data sent during that time can be lost.[2]

## Example `secrets.h`

```cpp
#pragma once

// WIFI
#define WIFI_SSID "twoje_wifi"
#define WIFI_PASS "twoje_haslo"

// APRS prywatne / personalne
#define APRS_PASS    "twoj_aprs_pass"
#define APRS_CALL_WX "APRS-15"
#define APRS_CALL_RS "APRS-12"

#define LAT_WX "5123.46N"
#define LON_WX "02123.50E"
#define LAT_RS "3231.44N"
#define LON_RS "02433.50E"

// ThingSpeak
#define THINGSPEAK_API_KEY_1 "twoj_key_1"
#define THINGSPEAK_API_KEY_2 "twoj_key_2"

// Windy Stations API v2
#define WINDY_STATION_ID "twoj_station_id"
#define WINDY_STATION_PASSWORD "twoj_station_password"
```

## Build and upload

1. Install Arduino IDE and ESP8266 board support.
2. Install the required libraries.
3. Put `esp8266_wx.ino` and `secrets.h` in the same sketch folder.
4. Select your ESP8266 board and COM port.
5. Compile and upload the sketch.
6. Open Serial Monitor at 115200 baud.

## Local access

After connecting to Wi-Fi, the sketch starts a local web server and prints:
- device IP address
- hostname
- mDNS address

Open the panel using the IP address or the local mDNS name shown in the serial log.

## Notes

- Pressure is validated before publishing to avoid obvious garbage readings.
- Windy upload is performed only when time is synced and measurements are valid.
- `#pragma once` in header files only prevents multiple inclusion during compilation; it does not hide secrets.
