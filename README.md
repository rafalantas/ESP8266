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
