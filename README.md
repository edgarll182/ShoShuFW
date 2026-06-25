# ShoShuFW

Firmware para **ShoShu**, un controlador personal para acuario basado en ESP32. El sistema integra pantalla OLED, encoder, RTC, sensor de temperatura, control de reles, configuracion WiFi y actualizaciones OTA manuales desde GitHub.

## Estado actual

- Version actual del firmware: **ShoShuV1.4**
- Plataforma: **ESP32 DOIT DevKit V1**
- Framework: **Arduino / PlatformIO**
- Actualizacion OTA: **manual desde menu**
- Repositorio OTA: **GitHub Releases**

## Funciones principales

- Control de 4 reles.
- Modos de rele:
  - apagado,
  - encendido,
  - automatico por horario,
  - automatico por temperatura,
  - intervalos.
- Sensor de temperatura DS18B20 con lectura no bloqueante.
- RTC DS3231 para conservar hora y fecha.
- Sincronizacion NTP cuando hay WiFi disponible.
- Pantalla OLED 128x64 por I2C.
- Menu con encoder rotativo.
- Configuracion WiFi desde portal cautivo.
- Portal WiFi protegido con contrasena.
- Diagnostico en pantalla.
- Actualizaciones OTA desde GitHub.
- Validacion OTA por tamano y SHA-256.
- Bloqueo por `minVersion` para evitar actualizar desde versiones demasiado antiguas.

## Componentes usados

| Componente | Uso |
|---|---|
| ESP32 DOIT DevKit V1 | Microcontrolador principal |
| OLED SSD1306 128x64 I2C | Pantalla del menu |
| Encoder EC11 | Navegacion del menu |
| DS18B20 | Sensor de temperatura |
| RTC DS3231 | Hora y fecha |
| Modulo de 4 reles | Control de salidas |
| Fuente 5V | Alimentacion del sistema |

## Mapa de pines

| Funcion | GPIO |
|---|---:|
| OLED/RTC SDA | 21 |
| OLED/RTC SCL | 22 |
| Encoder CLK | 32 |
| Encoder DT | 33 |
| Encoder SW | 25 |
| DS18B20 | 4 |
| Rele 1 | 16 |
| Rele 2 | 17 |
| Rele 3 | 18 |
| Rele 4 | 19 |

## WiFi

El sistema permite configurar una red WiFi desde el menu:

```text
Configuracion > WiFi > Escanear redes
```

Cuando se selecciona una red, ShoShu abre un portal WiFi temporal:

```text
SSID: ShoShu_Setup
PASS: ShoShu1234
```

El portal se usa solo para guardar las credenciales de la red local. Despues de guardar, el ESP32 reinicia y se conecta a la red configurada.

## OTA desde GitHub

Las actualizaciones se hacen manualmente desde:

```text
Configuracion > Actualizar
```

El ESP32 consulta este archivo:

```text
https://raw.githubusercontent.com/edgarll182/ShoShuFW/main/version.json
```

El archivo `firmware.bin` de cada version debe subirse como asset en GitHub Releases.

Ejemplo de `version.json`:

```json
{
  "version": "1.4",
  "build": "25/06/2026",
  "notes": "OTA log/minVersion",
  "minVersion": "1.3",
  "size": 1053536,
  "sha256": "262217b8533a31932b778fff9e842f0224b2d9b03ab9aa72c18c9032c39a9c7a",
  "url": "https://github.com/edgarll182/ShoShuFW/releases/download/v1.4/firmware.bin"
}
```

## Flujo para publicar una actualizacion

1. Cambiar la version interna del firmware en `src/OtaUpdate.cpp`.
2. Compilar en PlatformIO.
3. Tomar el archivo generado:

```text
.pio/build/esp32doit-devkit-v1/firmware.bin
```

4. Crear un release en GitHub, por ejemplo `v1.5`.
5. Subir `firmware.bin` al release.
6. Calcular el tamano y SHA-256 del archivo.
7. Actualizar `version.json` en la rama `main`.
8. Desde ShoShu, entrar a `Configuracion > Actualizar`.

## Librerias principales

- RTClib
- Adafruit GFX Library
- Adafruit SSD1306
- OneWire
- DallasTemperature
- WiFi
- WebServer
- DNSServer
- HTTPClient
- WiFiClientSecure
- Update
- Preferences

## Notas de seguridad

- OTA se activa solo de forma manual.
- El firmware descargado se valida por `size` y `sha256`.
- Si la descarga queda incompleta, se cancela la actualizacion.
- Si el SHA-256 no coincide, se cancela la actualizacion.
- Si el firmware no es valido, el ESP32 no cambia de version.
- Las configuraciones guardadas en `Preferences` se conservan durante OTA.

## Historial resumido

| Version | Cambios |
|---|---|
| V1.1 | Base con menus, relays, WiFi, RTC, temperatura y diagnostico |
| V1.2 | Primera OTA funcional desde GitHub Releases |
| V1.3 | Validacion OTA por `size` y `sha256`; pantalla Datos corregida |
| V1.4 | Registro de OTA exitosa despues del reinicio y soporte `minVersion` |

## Estado del proyecto

Proyecto personal en desarrollo y pruebas reales. Las funciones principales ya estan operativas, incluyendo OTA desde GitHub.
