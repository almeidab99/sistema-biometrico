# Biometric Access Control System with ESP32-CAM

[🇺🇸 English](#-english) | [🇪🇸 Español](#-español)

---

## 🇺🇸 English

Biometric access control project based on the **ESP32-CAM (AI-Thinker)**, combining **facial recognition** and **fingerprint reading** to authorize the opening of an electric lock (relay).

### Credits

This project is based on code by **Robot Zero One** ([original repository](https://github.com/robotzero1/esp32cam-access-control)) for the base facial recognition with ESP32-CAM, on top of which the extensions described in this document were developed (fingerprint, lock control, and full system integration).

### Description

The system allows registering and recognizing faces directly from the ESP32-CAM, while also validating a fingerprint through a sensor connected via UART. When verification is successful (face and/or fingerprint recognized), the system activates a relay that opens a lock for a configured time interval, then closes it automatically.

Face management (add, list, delete) is handled through a web interface served by the ESP32 itself, communicating in real time via WebSockets.

### Main features

- **Real-time facial recognition** using Espressif's `fd_forward` / `fr_forward` / `fr_flash` libraries.
- **Registration of up to 7 distinct faces**, stored in the ESP32's flash memory.
- **Fingerprint reading** via a UART-connected sensor (configurable pins).
- **Electric lock control** via relay, with timed opening.
- **Embedded web interface** (HTTP server + WebSockets) for:
  - Viewing the camera stream
  - Registering new faces
  - Listing and deleting saved faces
- **WiFi connection** for accessing the interface from the local network (with mDNS support).

### Required hardware

- ESP32-CAM (AI-Thinker model)
- Fingerprint sensor (UART connection)
- Relay module
- Electric lock / solenoid
- Suitable power supply for the whole setup

### Relevant connections

| Function                  | Pin / Configuration                             |
| -------------------------- | ----------------------------------------------- |
| Camera model               | `CAMERA_MODEL_AI_THINKER` (see `camera_pins.h`) |
| Flash LED                  | GPIO 4                                          |
| UART RX (fingerprint sensor) | GPIO 14                                       |
| UART TX (fingerprint sensor) | GPIO 15                                       |

> Camera pins are defined in `camera_pins.h` according to the configured model.

### 📦 Project structure

```
sistema_biometrico/
├── sistema_biometrico.ino   # Main sketch
├── camera_pins.h            # Pin configuration according to camera model
├── camera_index.h           # Embedded web interface (compressed HTML)
├── camera_index.html        # Web interface (readable source)
├── partitions.csv           # Flash memory partition scheme
└── secrets.h.example        # WiFi credentials template (copy to secrets.h)
```

### 🚀 Installation

1. Install [Arduino IDE](https://www.arduino.cc/en/software) (or PlatformIO) with ESP32 support.
2. Install the required libraries:
   - `ArduinoWebsockets`
   - `ESPmDNS`
   - Espressif's camera and facial recognition libraries (`esp32-camera`, face recognition)
3. Copy `secrets.h.example` to `secrets.h` and fill it in with your real WiFi credentials:
   ```cpp
   const char* ssid = "YOUR_WIFI_NETWORK";
   const char* password = "YOUR_PASSWORD";
   ```
4. Select the **AI Thinker ESP32-CAM** board in the IDE.
5. Connect the ESP32-CAM in programming mode (GPIO 0 to GND) and upload the sketch.
6. Restart the board in normal mode and check the IP assigned by the router (or use mDNS).
7. Access the web interface from your browser using that IP.

### Notes

- This is a **personal/educational** project, not intended as a production-grade security system.
- The relay and lock opening do not include any additional verification beyond facial/fingerprint recognition, so it should not be used as the sole security method in critical environments.
- WiFi credentials should never be pushed to the repository; use `secrets.h` (git-ignored) according to the included template.

### License

© Brian Almeida, 2024-2025. All rights reserved.

This repository is published for personal portfolio purposes. Use, copying, modification, or distribution is not authorized without the author's express permission.

The base facial recognition code on which this project was built belongs to Robot Zero One (see Credits section above).

---

## 🇪🇸 Español

Proyecto de control de acceso biométrico basado en **ESP32-CAM (AI-Thinker)**, que combina **reconocimiento facial** y **lectura de huella digital** para autorizar la apertura de una cerradura eléctrica (relay).

### Créditos

Este proyecto toma como punto de partida el código de **Robot Zero One** ([repositorio original](https://github.com/robotzero1/esp32cam-access-control)) para el reconocimiento facial base con ESP32-CAM, sobre el cual se desarrollaron las extensiones descritas en este documento (huella digital, control de cerradura, e integración completa del sistema).

### Descripción

El sistema permite registrar y reconocer rostros directamente desde el ESP32-CAM, y validar también una huella digital a través de un sensor conectado por UART. Cuando la verificación es exitosa (rostro y/o huella reconocidos), el sistema activa un relay que acciona la apertura de una cerradura durante un intervalo de tiempo configurado, y luego la cierra automáticamente.

La gestión de rostros (alta, listado, eliminación) se realiza desde una interfaz web servida por el propio ESP32, comunicándose en tiempo real mediante WebSockets.

### Funcionalidades principales

- **Reconocimiento facial** en tiempo real usando las librerías `fd_forward` / `fr_forward` / `fr_flash` de Espressif.
- **Registro de hasta 7 rostros** distintos, guardados en la memoria flash del ESP32.
- **Lectura de huella digital** mediante un sensor conectado por UART (pines configurables).
- **Control de cerradura eléctrica** vía relay, con apertura temporizada.
- **Interfaz web embebida** (servidor HTTP + WebSockets) para:
  - Ver el streaming de la cámara
  - Registrar nuevos rostros
  - Listar y eliminar rostros guardados
- **Conexión WiFi** para acceso a la interfaz desde la red local (con soporte mDNS).

### Hardware necesario

- ESP32-CAM (modelo AI-Thinker)
- Sensor de huella digital (conexión UART)
- Módulo relay
- Cerradura eléctrica / solenoide
- Fuente de alimentación adecuada para el conjunto

### Conexiones relevantes

| Función                    | Pin / Configuración                             |
| -------------------------- | ----------------------------------------------- |
| Modelo de cámara           | `CAMERA_MODEL_AI_THINKER` (ver `camera_pins.h`) |
| LED Flash                  | GPIO 4                                          |
| UART RX (sensor de huella) | GPIO 14                                         |
| UART TX (sensor de huella) | GPIO 15                                         |

> Los pines de la cámara están definidos en `camera_pins.h` según el modelo configurado.

### 📦 Estructura del proyecto

```
sistema_biometrico/
├── sistema_biometrico.ino   # Sketch principal
├── camera_pins.h            # Configuración de pines según modelo de cámara
├── camera_index.h           # Interfaz web embebida (HTML comprimido)
├── camera_index.html        # Interfaz web (fuente legible)
├── partitions.csv           # Esquema de particiones de memoria flash
└── secrets.h.example        # Plantilla de credenciales WiFi (copiar a secrets.h)
```

### 🚀 Instalación

1. Instalar [Arduino IDE](https://www.arduino.cc/en/software) (o PlatformIO) con soporte para ESP32.
2. Instalar las librerías necesarias:
   - `ArduinoWebsockets`
   - `ESPmDNS`
   - Librerías de cámara y reconocimiento facial de Espressif (`esp32-camera`, face recognition)
3. Copiar `secrets.h.example` a `secrets.h` y completar con tus credenciales WiFi reales:
   ```cpp
   const char* ssid = "TU_RED_WIFI";
   const char* password = "TU_PASSWORD";
   ```
4. Seleccionar la placa **AI Thinker ESP32-CAM** en el IDE.
5. Conectar el ESP32-CAM en modo programación (GPIO 0 a GND) y cargar el sketch.
6. Reiniciar la placa en modo normal y verificar la IP asignada por el router (o usar mDNS).
7. Acceder a la interfaz web desde el navegador usando esa IP.

### Notas

- Este es un proyecto **personal/educativo**, no está pensado como sistema de seguridad de producción.
- El relay y la apertura de la cerradura no incluyen verificación adicional más allá del reconocimiento facial/huella, por lo que no debe usarse como único método de seguridad en entornos críticos.
- Las credenciales WiFi nunca deben subirse al repositorio; usar `secrets.h` (ignorado por git) según la plantilla incluida.

### Licencia

© Brian Almeida, 2024-2025. Todos los derechos reservados.

Este repositorio se publica con fines de portafolio personal. No se autoriza su uso, copia, modificación o distribución sin permiso expreso del autor.

El código base de reconocimiento facial sobre el que se construyó este proyecto pertenece a Robot Zero One (ver sección de Créditos arriba).
