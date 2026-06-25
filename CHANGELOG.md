# Historial de cambios

## ShoShuV1.4

- Agrega registro de OTA exitosa despues del reinicio.
- Agrega soporte para `minVersion` en `version.json`.
- Mantiene los reles sin recalcular cambios mientras se escribe firmware por OTA.

## ShoShuV1.3

- Agrega validacion OTA por `size`.
- Agrega validacion OTA por `sha256`.
- Mejora deteccion de errores OTA:
  - JSON incompleto,
  - tamano distinto,
  - firmware grande,
  - descarga incompleta,
  - SHA-256 invalido.
- Corrige pantalla `Informacion > Datos` para separar firmware y build.

## ShoShuV1.2

- Agrega instalacion OTA real desde GitHub Releases.
- Muestra progreso de actualizacion en OLED.
- Reinicia automaticamente al terminar la actualizacion.
- Bloquea navegacion accidental durante OTA.

## ShoShuV1.1

- Base del proyecto.
- Menu principal en OLED.
- Control de 4 reles.
- Configuracion WiFi con portal cautivo.
- Lectura de temperatura DS18B20.
- RTC DS3231 con modo manual y NTP.
- Pantalla de diagnostico.
- Configuracion de brillo y apagado de pantalla.
