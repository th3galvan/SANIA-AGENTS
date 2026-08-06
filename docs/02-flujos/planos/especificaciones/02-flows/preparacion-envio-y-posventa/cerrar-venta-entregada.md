# Spec: Cerrar una venta entregada

Proyecto `sania-cerrar-venta-entregada`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

El cierre se resolverá por plataforma. Vinted se cerrará idempotentemente con el correo final TX-COMPLETE y su número de transacción. Wallapop permanecerá abierto después del correo de entrega y bloqueado para cierre mientras no se defina cómo recibe SANIA la evidencia del movimiento de venta en el monedero.

Cuando el comprador recibió el paquete, Víctor necesitó distinguir la entrega logística del cierre económico real y evitar que SANIA cerrara Wallapop sin una fuente final observable.

Criterios de éxito:
- Vinted se cerró una sola vez por número de transacción al reconocer TX-COMPLETE.
- El cierre de Vinted conservó número, fecha, título, precio del artículo, precio del envío y transferencia al saldo.
- El correo de entrega de Wallapop mantuvo la venta abierta y produjo un bloqueo explícito, no un cierre inferido.
- Cerrar una venta no convirtió el beneficio en definitivo mientras faltaran costes completos.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### SANIA cerró Vinted con TX-COMPLETE [con la app · origen: usuario]

- [tercero externo] Vinted envió el correo La transacción se ha completado con el código TX-COMPLETE.
- ⚠ Excepción: ¿El correo aportaba un número de transacción y los campos necesarios y pudo relacionarse con una venta?
    - si no:
        - [automático: código] SANIA conservó el correo, mantuvo la venta abierta y dejó el hecho sin aplicar ni completar datos por inferencia; T02-Q08 decidirá si además abre un ticket.
        - aquí termina este camino
    - camino normal: sí, SANIA continuó
- ⚑ Regla: ¿Ese número de transacción ya había cerrado la venta?
    - si sí:
        - [automático: código] SANIA no duplicó el cierre ni la transferencia económica.
        - aquí termina este camino
    - camino normal: no, era un cierre nuevo
- [automático: código] SANIA guardó número de transacción, fecha, título, precio del artículo, precio del envío y transferencia al saldo, y marcó la venta de Vinted como cerrada.
- [automático: código] El ingreso dejó de estar pendiente de cierre de plataforma, pero el beneficio siguió sin ser definitivo mientras faltaran costes completos.

### La entrega de Wallapop no cerró la venta [con la app · origen: usuario]

- [tercero externo] Wallapop envió el correo que confirma la entrega del paquete al comprador.
- [automático: código] SANIA registró la entrega y mantuvo la venta abierta porque el propio correo indica que el dinero estará disponible cuando el comprador confirme.
- [automático: código] SANIA dejó el cierre bloqueado por fuente no definida: el hecho final aparece en el monedero de Wallapop, pero no existe correo final ni un mecanismo autorizado para incorporarlo.
- [automático: código] SANIA mantuvo visible el bloqueo de cierre Wallapop sin dar por decidido que fuera un ticket, pedir un OK periódico ni aplicar una confirmación manual no acordada.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- Tres correos reales de Vinted mostraron TX-COMPLETE, número de transacción, fecha, título, precio del artículo, precio del envío y transferencia al saldo. Fixtures: Ventilador anti vaho — artículo 17,00 €, envío 2,65 €, transferencia 17,00 €; Airsoft face rack + balaclava — artículo 25,00 €, envío 3,85 €, transferencia 25,00 €; otra venta con el mismo título — artículo 24,95 €, envío 4,99 €, transferencia 24,95 €. [Migración: referencias históricas: E-LIVE-007, T06-Q01]
- El correo real de Wallapop para «gafas balísticas + máscara airsoft» registró una compra del 23/7/26 por 50,00 € y confirmó que el paquete fue entregado; dejó el dinero pendiente del OK del comprador y no constituyó cierre. [Migración: referencias históricas: E-LIVE-008, T06-Q02]
- La captura del monedero mostró ventas, retiradas, recargas y reembolsos por anuncio, pero SANIA todavía no dispone de una fuente definida para recibir esos movimientos. [Migración: referencias históricas: E-LIVE-009, T06-Q02, X-LIVE-005]

## 5. Reglas de negocio

### G-12: Vinted cierra con TX-COMPLETE

El correo final cierra la venta una sola vez por número de transacción y conserva fecha, título e importes. [Migración: identificador histórico G-LIVE-011; referencias históricas: D-LIVE-004, T06-Q01]

### G-13: Wallapop entregado no cierra

El correo de entrega solo prueba entrega logística; la venta queda abierta hasta un hecho final que SANIA pueda recibir de una fuente todavía no definida. [Migración: identificador histórico G-LIVE-012; referencias históricas: D-LIVE-005, X-LIVE-005, T06-Q02]

### G-14: Cerrar la plataforma no completa el beneficio

El importe de venta se registra, pero el beneficio no se considera definitivo sin costes reales completos. [Migración: identificador histórico D-LIVE-030]

## 6. Estados

### venta Vinted

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| entregada y abierta | recibir y validar TX-COMPLETE → pasa a 'cerrada' |
| cerrada | registrar los importes sin duplicarlos y mantener el beneficio provisional si faltan costes |

### venta Wallapop

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| entregada y abierta | detectar que falta una fuente final observable → pasa a 'bloqueada para cierre' |
| bloqueada para cierre | mantener visible el bloqueo hasta decidir la fuente del movimiento del monedero y si se representa como ticket |

## 7. Datos e integraciones

- Habla con **Gmail**: reconocer el correo TX-COMPLETE de Vinted y el correo de entrega no final de Wallapop
- Habla con **Telegram**: avisar del bloqueo o de un ticket, sin inventar una confirmación periódica de cierre
- Habla con **monedero de Wallapop**: fuente final necesaria pero todavía no autorizada ni conectada

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Cerrar Wallapop únicamente porque el paquete figuró como entregado.
- Pedir a Víctor un OK cada 48 horas: ni la confirmación ni la cadencia fueron acordadas.
- Inferir un movimiento del monedero a partir del precio del correo inicial.
- Convertir automáticamente el cierre de plataforma en beneficio definitivo.
- Automatizar devoluciones o extravíos sin un caso real verificado.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T06-Q01] ¿Existen variantes de idioma o plantilla de TX-COMPLETE y qué campos serán obligatorios?
- [T06-Q02, X-LIVE-005] ¿Cómo llegará a SANIA el movimiento final del monedero de Wallapop: confirmación manual o fuente de lectura autorizada?
- [T06-Q03] ¿Qué se hace y tras cuánto tiempo si falta la señal final de plataforma?
- [T06-Q04] ¿Qué incidencias concretas bloquean el cierre y qué evidencia las desbloquea?
- [T06-Q05, T06-Q06, T06-Q10] ¿Qué costes y reglas faltan para convertir el resultado de una venta cerrada en beneficio definitivo?

