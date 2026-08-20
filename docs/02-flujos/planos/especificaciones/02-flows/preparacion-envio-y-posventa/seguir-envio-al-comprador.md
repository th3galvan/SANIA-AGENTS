# Spec: Seguir el envío al comprador

Proyecto `sania-seguir-envio-al-comprador`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

SANIA relacionará con la venta correcta únicamente correos pertenecientes a plantillas reales aportadas y validadas, conservará la evidencia y aplicará cada hecho una sola vez. LIVE aportó un correo de entrega de Wallapop, pero no plantillas reales de admisión, tránsito, intento fallido o incidencia por transportista. Un estado logístico de entregado no cerrará por sí solo la venta, y no se usará el umbral de 48 horas porque la entrevista no fijó ninguna cadencia.

Cuando llegó una evidencia logística, Víctor necesitó que SANIA actualizara solo el envío demostrado, mantuviera aparte el cierre de la venta y dejara visible cualquier lectura dudosa.

Criterios de éxito:
- Cada correo reconocido actualizó como máximo una vez el envío relacionado.
- Un correo desconocido, contradictorio o imposible de relacionar no inventó una transición y quedó sin aplicar; T02-Q08 decidirá cuándo además abre un ticket.
- La entrega logística no se confundió con el OK final del comprador ni con la disponibilidad del dinero.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### SANIA actualizó el trayecto demostrado por correos reconocidos [con la app · origen: usuario]

- [tercero externo] SANIA recibió un correo perteneciente a una plantilla logística real previamente aportada y validada. En LIVE solo se examinó una plantilla de entrega de Wallapop; admisión, tránsito, intento fallido e incidencia siguen sin corpus real.
- ⚠ Excepción: ¿La plantilla y sus identificadores eran reconocidos y permitían relacionar el hecho con un solo envío?
    - si no:
        - [automático: código] SANIA conservó el correo y no aplicó la transición. T02-Q08 dejó pendiente si el caso abre un ticket o permanece solo sin aplicar.
        - aquí termina este camino
    - camino normal: sí, SANIA continuó
- ⚑ Regla: ¿El mismo hecho externo ya se había aplicado?
    - si sí:
        - [automático: código] SANIA conservó el envío sin repetir el evento ni sus efectos.
        - aquí termina este camino
    - camino normal: no, se registró el nuevo evento
- [automático: código] SANIA actualizó el estado logístico demostrado y conservó el correo y su fecha como evidencia.
- ⚑ Regla: ¿El hecho era una entrega al comprador?
    - si sí:
        - [automático: código] SANIA dejó el envío como entregado, pero mantuvo la venta abierta hasta la señal final específica de la plataforma.
        - …y vuelve al flujo
    - camino normal: no, el seguimiento continuó según los eventos disponibles

### Víctor atendió manualmente un tracking sin cambios [con la app · origen: usuario]

- [persona] Víctor consultó manualmente el tracking en la app o web cuando faltaban confirmaciones. · Víctor
- [persona] Si había una incidencia, esperó; si el estado no cambiaba, contactó al transportista e informó al comprador. · Víctor
- [automático: código] SANIA mantuvo el caso pendiente porque no se definió cuántos días esperar ni cuándo convertirlo en extravío.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- Después de la entrega al transportista, Víctor suele avisar al comprador, espera las evidencias y consulta manualmente el tracking cuando falta una confirmación; en Wallapop el tracking puede ser difícil de identificar. [Migración: referencias históricas: E-LIVE-001, T05-Q04, T06-Q03]
- El correo real de Wallapop informó de que el paquete fue entregado, pero el dinero seguía pendiente del OK del comprador. [Migración: referencias históricas: E-LIVE-008, T06-Q02]

## 5. Reglas de negocio

### G-86: Cada hecho logístico se aplica una sola vez

Un correo repetido o reprocesado no duplica estados, movimientos ni avisos. [Migración: identificador histórico G-LIVE-001; referencias históricas: T02-Q05]

### G-87: Entregado en Wallapop no significa cerrado

El correo de paquete entregado prueba la entrega logística, pero la venta y el ingreso continúan abiertos. [Migración: identificador histórico G-LIVE-012; referencias históricas: D-LIVE-005, T06-Q02]

## 6. Estados

### envío

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| pendiente de evidencia de admisión | recibir un correo reconocido de admisión → pasa a 'admitido' · dejar sin aplicar una evidencia desconocida o contradictoria; decidir después si requiere ticket |
| admitido | recibir evidencias posteriores → pasa a 'en tránsito o entregado' |
| entregado | continuar en la actividad de cierre específica de la plataforma; no cerrar la venta aquí |

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| envío | venta, tracking cuando exista, transportista cuando la evidencia lo aporte, estado logístico demostrado, eventos y correos originales, fechas, tickets abiertos | correos reconocidos de la plataforma o del transportista; no se definió cómo SANIA recibe o documenta las actuaciones manuales de Víctor |

- Habla con **Gmail**: recibir y conservar evidencias logísticas reconocidas
- Habla con **Telegram**: avisar de incidencias, tickets y acciones humanas concretas sin imponer una cadencia no acordada

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Actualizar un estado desde un correo que no pudo reconocerse o relacionarse con seguridad.
- Pedir confirmaciones manuales genéricas para suplir cualquier evidencia ausente.
- Avisar automáticamente a las 48 horas: ese umbral no fue acordado.
- Cerrar la venta porque el envío figure como entregado.
- Declarar extravío o automatizar su resolución antes de disponer de un procedimiento real.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T02-Q04, T05-Q04] ¿Qué plantillas reales cubren admisión, tránsito, intento fallido, incidencia y entrega por plataforma y transportista?
- [T02-Q05, T05-Q06] ¿Qué prioridad y ticket se aplican cuando un correo llega tarde, duplicado o contradice una confirmación anterior?
- [T02-Q06] ¿Qué patrones y campos obligatorios hacen determinista cada plantilla logística?
- [T05-Q08] ¿Existe alguna acción concreta a las 48 horas? La entrevista no fijó ninguna.
- [T05-Q09, T06-Q03] ¿Cuánto se espera ante un tracking sin cambios antes de contactar al transportista y avisar al comprador?
- [T06-Q04, T11-Q02] ¿Qué incidencia bloquea el cierre y cuándo se abre un ticket de extravío?

