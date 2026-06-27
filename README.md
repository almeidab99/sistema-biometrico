# Sistema Biométrico con ESP32-CAM

Proyecto de control de acceso biométrico basado en **ESP32-CAM (AI-Thinker)**, que combina **reconocimiento facial** y **lectura de huella digital** para autorizar la apertura de una cerradura eléctrica (relay).

## Créditos

Este proyecto toma como punto de partida el código de **Robot Zero One** ([repositorio original](https://github.com/robotzero1/esp32cam-access-control)) para el reconocimiento facial base con ESP32-CAM, sobre el cual se desarrollaron las extensiones descritas en este documento (huella digital, control de cerradura, e integración completa del sistema).

## Descripción

El sistema permite registrar y reconocer rostros directamente desde el ESP32-CAM, y validar también una huella digital a través de un sensor conectado por UART. Cuando la verificación es exitosa (rostro y/o huella reconocidos), el sistema activa un relay que acciona la apertura de una cerradura durante un intervalo de tiempo configurado, y luego la cierra automáticamente.

La gestión de rostros (alta, listado, eliminación) se realiza desde una interfaz web servida por el propio ESP32, comunicándose en tiempo real mediante WebSockets.

## Funcionalidades principales

- **Reconocimiento facial** en tiempo real usando las librerías `fd_forward` / `fr_forward` / `fr_flash` de Espressif.
- **Registro de hasta 7 rostros** distintos, guardados en la memoria flash del ESP32.
- **Lectura de huella digital** mediante un sensor conectado por UART (pines configurables).
- **Control de cerradura eléctrica** vía relay, con apertura temporizada.
- **Interfaz web embebida** (servidor HTTP + WebSockets) para:
  - Ver el streaming de la cámara
  - Registrar nuevos rostros
  - Listar y eliminar rostros guardados
- **Conexión WiFi** para acceso a la interfaz desde la red local (con soporte mDNS).

## Hardware necesario

- ESP32-CAM (modelo AI-Thinker)
- Sensor de huella digital (conexión UART)
- Módulo relay
- Cerradura eléctrica / solenoide
- Fuente de alimentación adecuada para el conjunto

## Conexiones relevantes

| Función                    | Pin / Configuración                             |
| -------------------------- | ----------------------------------------------- |
| Modelo de cámara           | `CAMERA_MODEL_AI_THINKER` (ver `camera_pins.h`) |
| LED Flash                  | GPIO 4                                          |
| UART RX (sensor de huella) | GPIO 14                                         |
| UART TX (sensor de huella) | GPIO 15                                         |

> Los pines de la cámara están definidos en `camera_pins.h` según el modelo configurado.

## 📦 Estructura del proyecto

```
sistema_biometrico/
├── sistema_biometrico.ino   # Sketch principal
├── camera_pins.h            # Configuración de pines según modelo de cámara
├── camera_index.h           # Interfaz web embebida (HTML comprimido)
├── camera_index.html        # Interfaz web (fuente legible)
├── partitions.csv           # Esquema de particiones de memoria flash
└── secrets.h.example        # Plantilla de credenciales WiFi (copiar a secrets.h)
```

## 🚀 Instalación

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

## Notas

- Este es un proyecto **personal/educativo**, no está pensado como sistema de seguridad de producción.
- El relay y la apertura de la cerradura no incluyen verificación adicional más allá del reconocimiento facial/huella, por lo que no debe usarse como único método de seguridad en entornos críticos.
- Las credenciales WiFi nunca deben subirse al repositorio; usar `secrets.h` (ignorado por git) según la plantilla incluida.

## Licencia

© Brian Almeida, 2024-2025. Todos los derechos reservados.

Este repositorio se publica con fines de portafolio personal. No se autoriza su uso, copia, modificación o distribución sin permiso expreso del autor.

El código base de reconocimiento facial sobre el que se construyó este proyecto pertenece a Robot Zero One (ver sección de Créditos arriba).
