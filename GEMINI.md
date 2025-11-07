# GEMINI.md: M5Cardputer WebRadio FR

## Project Overview

This project is a web radio application specifically designed for the M5Stack Cardputer device. It allows users to stream and listen to various French-language internet radio stations.

**Key Technologies:**
*   **Framework:** Arduino
*   **Build System:** PlatformIO
*   **Core Hardware:** M5Stack Cardputer (ESP32-S3)
*   **Main Libraries:**
    *   `M5Unified`: Provides a unified API for M5Stack devices.
    *   `M5Cardputer`: Specific drivers and helpers for the Cardputer hardware.
    *   `ESP8266Audio`: A library for decoding and playing various audio formats, used here for MP3 streams.

**Architecture:**
*   The application runs on the ESP32-S3 microcontroller of the Cardputer.
*   On startup, it connects to a pre-configured WiFi network.
*   It fetches a dynamic list of radio stations by making an HTTP GET request to an external server (`http://philae.synology.me/~admin/arduino/radio_dico.php`). This avoids the hardware conflicts between WiFi and the SD card that can occur on this device.
*   The `ESP8266Audio` library handles the streaming and decoding of the selected MP3 radio station.
*   The device's built-in display is used to show the current station, stream title, and a real-time audio visualizer (FFT spectrum).
*   User interaction is managed via the Cardputer's keyboard and built-in button for changing stations and controlling volume.
*   The last played station index and volume level are persisted across reboots using the ESP32's `Preferences` library (EEPROM).

## Building and Running

This project is configured to be built and managed using the PlatformIO IDE, typically within Visual Studio Code.

*   **Build the project:**
    ```shell
    pio run
    ```

*   **Upload to the Cardputer:**
    Connect the Cardputer via USB and run the following command.
    ```shell
    pio run --target upload
    ```

*   **Monitor Serial Output:**
    To view logs and debugging information from the device:
    ```shell
    pio device monitor
    ```

## Development Conventions

*   The main application logic is contained within the `src/webradio.ino` file.
*   All project dependencies are managed via the `lib_deps` section in the `platformio.ini` file.
*   Hardware-specific initialization and configuration for the Cardputer are handled using the `M5Unified` and `M5Cardputer` libraries.
*   The WiFi credentials are set using the `CardWifiSetup.h` header, which is not included in the repository and must be created by the user.
*   The list of radio stations is intentionally kept external and fetched at runtime to allow for easy updates without needing to re-flash the firmware.
