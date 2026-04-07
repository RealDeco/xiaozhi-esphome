# Xiaozhi Ball V2 ESPHome Notes

## Origen Del Proyecto

El codigo original de este proyecto parte de:

- proyecto principal: `https://github.com/RealDeco/xiaozhi-esphome`
- origen concreto del dispositivo: `https://github.com/RealDeco/xiaozhi-esphome/tree/main/devices/Spotpear%20Balls`

Sobre esa base se han hecho adaptaciones, correcciones y cambios de estructura para este repositorio.

## Overview

Este proyecto contiene configuraciones ESPHome para un dispositivo tipo "ball" con:

- ESP32-S3
- pantalla redonda `240x240`
- LED RGB de estado
- microfono I2S
- altavoz con codec ES8311
- integracion con Home Assistant Voice Assistant
- wake word local o via Home Assistant
- botones fisicos y modo noche
- pantalla tactil en la variante completa

La configuracion ahora esta separada en base comun + overlays:

- [`ball_v2_base.yaml`](/home/carlos/bolas_ia/ball_v2_base.yaml): toda la logica comun compartida.
- [`ball_v2_lite.yaml`](/home/carlos/bolas_ia/ball_v2_lite.yaml): wrapper para hardware sin touch ni bateria.
- [`ball_v2_full_hw.yaml`](/home/carlos/bolas_ia/ball_v2_full_hw.yaml): extras de hardware completo.
- [`ball_v2.yaml`](/home/carlos/bolas_ia/ball_v2.yaml): wrapper de la variante completa, que carga la base comun y el overlay full.

Esto reduce duplicacion: si cambias audio, UI, wake word, night mode o integracion con Home Assistant, normalmente lo haces una sola vez en la base.

Importante:

- [`ball_v2_base.yaml`](/home/carlos/bolas_ia/ball_v2_base.yaml) es un paquete base, no un YAML standalone para flashear o validar por si solo
- valida y compila siempre desde [`ball_v2_lite.yaml`](/home/carlos/bolas_ia/ball_v2_lite.yaml) o [`ball_v2.yaml`](/home/carlos/bolas_ia/ball_v2.yaml)


## Funciones Principales

El dispositivo puede:

- actuar como satelite de voz para Home Assistant
- mostrar diferentes pantallas segun el estado del asistente
- reproducir sonido de arranque
- mostrar temporizadores del asistente
- mostrar informacion en reposo:
  - reloj digital
  - temperatura
  - humedad
  - proxima alarma
- entrar en `Night Mode`
- abrir una pantalla tactil de ajustes en la variante completa

## Estados Visuales

La pantalla cambia segun el estado:

- `idle`
- `listening`
- `thinking`
- `replying`
- `muted`
- `error`
- `no_wifi`
- `no_ha`
- `timer_finished`
- `settings_page` en la variante con tactil

En la version actual del `idle`:

- el avatar se muestra mientras hablas y durante unos segundos mas al volver a reposo
- despues se muestra un panel limpio con fondo configurable y texto configurable
- si hay un temporizador activo, la cuenta atras sustituye temporalmente a la proxima alarma

Por defecto:

- fondo del panel idle: negro
- color del texto: blanco

## Controles

### Boton fisico

Segun la configuracion actual:

- pulsacion corta: hablar / parar escucha / cancelar alarma de temporizador
- triple pulsacion rapida: alterna `Night Mode`
- pulsacion muy larga: `factory reset`

### Pantalla tactil

En la variante completa:

- swipe a la izquierda desde `idle`: abre la pantalla de ajustes
- swipe a la derecha: vuelve a la pantalla normal
- si pasan `5s` sin tocar nada, la pantalla de ajustes se cierra sola

Dentro de la pantalla de ajustes:

- izquierda: volumen
- derecha: brillo
- ambos se muestran como barras verticales con porcentaje
- deslizar arriba o abajo en cada mitad ajusta el nivel correspondiente

Fuera de esa pantalla:

- toque corto: misma accion principal que el boton fisico
- doble toque: alterna `Mute`

### Night Mode

`Night Mode` puede activarse:

- desde Home Assistant
- con triple pulsacion rapida del boton

Cuando se activa:

- apaga la pantalla
- apaga el LED
- detiene reproduccion/anuncios en curso
- apaga el amplificador (`speaker_enable`)

Notas:

- `Night Mode` ya no cambia persistentemente el switch `Startup sound`
- simplemente bloquea el sonido de arranque mientras el modo noche este activo

## Variantes De Hardware

### Estructura de mantenimiento

#### Base comun

Edita [`ball_v2_base.yaml`](/home/carlos/bolas_ia/ball_v2_base.yaml) para cambios compartidos como:

- voice assistant
- wake word
- scripts y automatizaciones
- night mode
- display
- dashboard idle
- LED de estado
- sonido
- integracion con Home Assistant

#### Overlay full

Edita [`ball_v2_full_hw.yaml`](/home/carlos/bolas_ia/ball_v2_full_hw.yaml) solo para hardware adicional:

- bateria
- tactil
- gestos tactiles y pantalla de ajustes
- sensores o controles que solo existan en esa placa

#### Wrappers

Los wrappers solo definen identidad y ensamblan paquetes:

- [`ball_v2_lite.yaml`](/home/carlos/bolas_ia/ball_v2_lite.yaml)
- [`ball_v2.yaml`](/home/carlos/bolas_ia/ball_v2.yaml)


### Variante completa

Usa [`ball_v2.yaml`](/home/carlos/bolas_ia/ball_v2.yaml) cuando tu dispositivo tenga:

- tactil
- bateria
- o quieras mantener compatibilidad con ese hardware

### Variante lite

Usa [`ball_v2_lite.yaml`](/home/carlos/bolas_ia/ball_v2_lite.yaml) cuando tu dispositivo no tenga:

- pantalla tactil
- bateria

Esto evita:

- errores del controlador tactil
- lecturas ADC innecesarias
- spam de log por hardware no presente

## Home Assistant Integration

### Que datos puede mostrar en idle

El dispositivo puede mostrar en reposo:

- reloj
- temperatura
- humedad
- proxima alarma

En Home Assistant aparecen switches para activar o desactivar cada uno:

- `Show Idle Clock`
- `Show Idle Temperature`
- `Show Idle Humidity`
- `Show Idle Next Alarm`

Esos switches solo controlan si se muestra el dato.

La fuente del dato ya no se lee directamente desde sensores globales de HA. Ahora cada bola expone tres entidades `text` editables:

- `Idle Temperature Text`
- `Idle Humidity Text`
- `Idle Next Alarm Text`

Home Assistant decide que mostrar en cada bola y escribe el texto ya formateado en esas entidades.

## Opcion Recomendada: Publicar Texto Desde Home Assistant

Este enfoque evita que varias bolas compartan accidentalmente los mismos datos.

La idea es:

1. cada bola expone sus propios `text`
2. Home Assistant formatea el contenido
3. un script o automatizacion actualiza cada bola concreta

## Ejemplo De Automatizacion En Home Assistant

Ejemplo generico para una bola concreta usando entidades `text` expuestas por ESPHome:

```yaml
alias: Actualizar idle bola ejemplo
triggers:
  - trigger: homeassistant
    event: start
  - trigger: state
    entity_id:
      - sensor.temperatura_ejemplo
      - sensor.humedad_ejemplo
      - sensor.proxima_alarma_ejemplo
actions:
  - delay: "00:00:10"

  - action: text.set_value
    target:
      entity_id: text.bola_ejemplo_idle_temperature_text
    data:
      value: >-
        {% set v = states('sensor.temperatura_ejemplo') %}
        {{ '' if v in ['unknown', 'unavailable', 'none', ''] else '%.2f °C' | format(v | float) }}

  - action: text.set_value
    target:
      entity_id: text.bola_ejemplo_idle_humidity_text
    data:
      value: >-
        {% set v = states('sensor.humedad_ejemplo') %}
        {{ '' if v in ['unknown', 'unavailable', 'none', ''] else '%.0f %%' | format(v | float) }}

  - action: text.set_value
    target:
      entity_id: text.bola_ejemplo_idle_next_alarm_text
    data:
      value: >-
        {% set v = states('sensor.proxima_alarma_ejemplo') %}
        {{ '' if v in ['unknown', 'unavailable', 'none', ''] else as_timestamp(v) | timestamp_custom('%d/%m %H:%M', true) }}
mode: restart
```

Notas:

- el `delay` inicial ayuda a que el nodo ESPHome ya este disponible cuando arranca Home Assistant
- la entidad de alarma del movil Android suele venir del Companion App; sustituye `sensor.proxima_alarma_ejemplo` por la tuya real
- el formato final del texto lo controla Home Assistant

## Configuracion Correspondiente En ESPHome

La logica comun ya crea estas entidades `text` en cada dispositivo:

- `Idle Temperature Text`
- `Idle Humidity Text`
- `Idle Next Alarm Text`

La pantalla `idle` usa esos textos directamente. Eso significa:

- Home Assistant puede mandar valores distintos a cada bola
- el formato final lo controlas en HA
- el ESP no necesita conocer el sensor real de origen

## Layout Idle Actual

El panel `idle` muestra:

- temperatura arriba con icono de termometro
- humedad debajo con icono de gota
- hora centrada y mas grande en el medio
- si hay temporizador activo, cuenta atras bajo la hora
- alarma debajo con icono de campana amarilla
- en la variante completa, porcentaje de bateria debajo de la alarma con icono de bateria

Las fuentes del panel `idle` ya incluyen `°` y `º`.

## Otras Opciones Configurables En El YAML

Algunas opciones utiles en la cabecera del YAML:

```yaml
idle_dashboard_background_color: "000000"
idle_dashboard_text_color: "FFFFFF"
idle_avatar_hold_ms: "2500"
```

Sirven para:

- color de fondo del panel de reposo
- color del texto del panel de reposo
- tiempo durante el que se sigue viendo el avatar al volver a `idle`

## Validacion

Para validar una configuracion:

```bash
docker exec -it esphome esphome config ball_v2.yaml
docker exec -it esphome esphome config ball_v2_lite.yaml
```

Para compilar y subir:

```bash
docker exec -it esphome esphome run ball_v2.yaml
docker exec -it esphome esphome run ball_v2_lite.yaml
```

## Problemas Conocidos

### Strapping pins

El log puede avisar sobre:

- `GPIO45`
- `GPIO46`

Son `strapping pins`. No es necesariamente un error, pero hay que usarlos con cuidado porque participan en el arranque del ESP32-S3.

### Carrera de audio al arrancar

Puede haber conflicto entre:

- sonido de arranque
- wake word
- inicializacion de microfono I2S

Ya se endurecio el arranque del wake word para no iniciar mientras el `media_player` no este realmente en `IDLE`, pero sigue siendo un area sensible.

### Recursos remotos en build time

Las imagenes y sonidos del proyecto se obtienen en build time desde el proyecto original y quedan embebidos en el firmware.

Eso significa:

- en ejecucion no hace falta descargar esos recursos de internet
- en runtime la bola solo necesita hablar con Home Assistant y con los servicios locales que este use

### Restauracion temprana de textos

Las entidades `Idle Temperature Text`, `Idle Humidity Text` e `Idle Next Alarm Text` no restauran su valor al arrancar.

Motivo:

- restaurar esos textos en `setup()` disparaba `draw_display`
- eso podia tocar LED/display demasiado pronto
- y provocar boot loop o `safe_mode`

Por eso los textos deben ser repoblados por Home Assistant al iniciar.

### Hardware no presente

Si una variante no tiene tactil o bateria, conviene usar la variante `lite` para evitar:

- errores del controlador tactil
- lecturas ADC inutiles
- spam en logs

## Archivos Relacionados

- [`ball_v2_base.yaml`](/home/carlos/bolas_ia/ball_v2_base.yaml)
- [`ball_v2.yaml`](/home/carlos/bolas_ia/ball_v2.yaml)
- [`ball_v2_lite.yaml`](/home/carlos/bolas_ia/ball_v2_lite.yaml)
- [`ball_v2_full_hw.yaml`](/home/carlos/bolas_ia/ball_v2_full_hw.yaml)
- [`SESSION_NOTES.md`](/home/carlos/bolas_ia/SESSION_NOTES.md)
- [`secrets.yaml`](/home/carlos/bolas_ia/secrets.yaml)
