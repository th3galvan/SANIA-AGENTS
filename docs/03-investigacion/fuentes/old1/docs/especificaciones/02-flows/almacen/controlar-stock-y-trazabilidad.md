# Spec: Controlar el stock y la trazabilidad

Proyecto `sania-controlar-stock-y-trazabilidad`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

## 1. Propósito

Esta actividad conserva la historia de cada unidad de venta desde el pedido, paquete y comprobación física hasta sus anuncios, reserva, venta, incidencia o ajuste. La referencia de unidad es pública, alfanumérica y de tres caracteres; no se registran ubicaciones físicas detalladas y la identidad exacta de unidades idénticas sigue abierta.

Cuando una unidad cambió de situación, Víctor necesitó que SANIA supiera de qué comprobación física procedía, qué referencia pública la representaba, dónde estaba comprometida y quién corrigió cada dato.

## 2. Actores y vocabulario

- **Víctor**: consultó el stock, localizó visualmente los productos, confirmó hechos físicos y explicó las correcciones excepcionales

- "referencia de unidad": código alfanumérico único de tres caracteres por unidad, visible al final del título del anuncio y conservado en la historia interna
- "unidad disponible": unidad de Stock para venta creada después de Todo correcto y no reservada, vendida ni bloqueada
- "ubicación": estantería, caja o posición física exacta; no se registra en la primera versión porque el stock actual se guarda en una caja bajo el escritorio
- "identidad física exacta": correspondencia entre una referencia lógica y un ejemplar material concreto; no está resuelta para unidades idénticas sin etiquetas ni diferencias visibles

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### SANIA incorporó una unidad físicamente confirmada [con la app]

- ⚑ Regla: ¿Existió Todo correcto después de la comprobación física?
    - si no, solo existió tracking entregado o No OK:
        - [automático: código] SANIA no creó la unidad ni aumentó el stock.
        - aquí termina este camino
    - camino normal: sí, continuó
- ⚑ Regla: ¿El producto estaba clasificado como Stock para venta?
    - si no, era Compra personal:
        - [automático: código] SANIA excluyó la compra del inventario de venta.
        - aquí termina este camino
    - camino normal: sí, SANIA creó la unidad
- [automático: código] SANIA vinculó la unidad con pedido, paquete, producto, variante y confirmación física, cuando esos datos estuvieron demostrados.
- [automático: código] SANIA asignó una referencia alfanumérica única de tres caracteres y la dejó disponible sin ubicación física detallada.

### SANIA mantuvo disponibilidad, anuncios y reserva de una unidad [con la app]

- [automático: código] SANIA relacionó la unidad con sus anuncios por plataforma; cada anuncio representó una unidad y su título terminó con la referencia pública.
- ⚑ Regla: ¿La unidad estaba físicamente disponible y no reservada?
    - si no:
        - [automático: código] SANIA excluyó la unidad del recuento disponible sin crear stock negativo.
        - …y vuelve al flujo
    - camino normal: sí, contó como disponible
- [tercero externo] Llegó un correo reconocido de venta con el título del anuncio y, cuando se conserva, los identificadores externos disponibles.
- ⚑ Regla: ¿El mismo correo o hecho externo ya había sido aplicado?
    - si sí:
        - [automático: código] SANIA conservó el evento existente y no duplicó la reserva, la venta ni el movimiento de stock.
        - aquí termina este camino
    - camino normal: no, SANIA continuó
- ⚠ Excepción: ¿El título conservó un sufijo válido de tres caracteres que identificaba una unidad exacta disponible?
    - si no: referencia ausente, alterada, desconocida o unidad no disponible:
        - [automático: código] SANIA abrió una conciliación, no cambió el stock y no eligió otra unidad por FIFO. T01-Q02 y X-LIVE-010 exigen validar el sufijo en correos y movimientos reales.
        - aquí termina este camino
    - camino normal: sí, SANIA pudo reservar esa referencia
- [automático: código] SANIA reservó la unidad exacta del anuncio. Si la misma última unidad estaba anunciada en la otra plataforma, creó el aviso para que Víctor retirara manualmente el anuncio alternativo.
- [automático: código] La concurrencia de dos correos casi simultáneos sobre la última unidad sigue sin regla completa; SANIA nunca puede inventar stock ni dejarlo negativo.

### Víctor corrigió un dato de stock sin borrar la historia [con la app]

- [persona] Víctor indicó el dato erróneo, el valor correcto y el motivo. · Víctor
- [automático: código] SANIA añadió un evento con valor anterior, valor nuevo, actor, fecha y hora y motivo, conservando el evento original.
- [automático: código] El flujo concreto para deshacer Todo correcto, corregir la clasificación de pedidos existentes o resolver efectos económicos sigue pendiente.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- En pedidos divididos o consolidados solo lo físicamente comprobado puede originar unidades; el pedido permanece abierto mientras queden paquetes pendientes. [T02-Q03, T02-Q09, D-LIVE-017, G-LIVE-002]
- Cuando Víctor detecta un fallo de stock, hoy ajusta manualmente el Excel; SANIA debe convertir esa corrección en un evento auditable. [T03-Q04, T04-Q01]
- Si la última unidad está anunciada en Wallapop y Vinted, el primer correo la reserva y SANIA pide retirar manualmente el otro anuncio. [T01-Q08, T04-Q04, D-LIVE-001, D-LIVE-009, G-LIVE-008]

## 5. Reglas de negocio

### G-LIVE-001: Idempotencia de hechos de stock

Un mismo correo o hecho externo se aplica una sola vez; reprocesarlo no duplica reservas, ventas ni movimientos.

### G-LIVE-002: Entrega no equivale a stock

El tracking entregado no crea una unidad disponible; se exige Todo correcto.

### G-LIVE-003: Todo correcto crea unidades

Solo después de comprobar producto y cantidades se registran unidades de Stock para venta y se lanzan sus tareas de anuncio.

### G-LIVE-005: Un anuncio por unidad

Cada anuncio representa una única unidad física.

### G-LIVE-006: Referencia pública de tres caracteres

La referencia alfanumérica de tres caracteres identifica la unidad en SANIA y aparece como sufijo del título.

### G-LIVE-007: Reserva de la unidad exacta

El correo de venta reserva la referencia contenida en el anuncio; no se aplica FIFO automático.

### G-LIVE-008: Primer correo sobre una unidad compartida

Si una unidad está publicada en dos plataformas, el primer correo la reserva y genera el aviso de retirada manual del otro anuncio.

### G-LIVE-016: Compra personal fuera del stock

Las compras personales no entran en el inventario de venta.

### G-LIVE-017: Clasificación corregible

La clasificación recordada por producto puede cambiarse manualmente.

### G-LIVE-018: Sin ubicación detallada

La primera versión conserva el estado del stock, no la posición física exacta.

### CORRECCION-AUDITABLE: Correcciones con historia

Toda corrección conserva antes, después, actor, fecha y hora y motivo sin sobrescribir el historial.

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| unidad de inventario | referencia alfanumérica pública de tres caracteres, producto y variante, pedido, paquete y línea cuando sean identificables, confirmación física de origen, situación de disponibilidad, anuncios por plataforma, reserva y venta, incidencias, historial | Todo correcto sobre Stock para venta |
| evento de trazabilidad | unidad o entidad afectada, tipo de hecho, fecha y hora, actor, origen, valor anterior, valor nuevo, motivo | hechos de correo, confirmaciones físicas y correcciones |

- Habla con **Dar entrada a las unidades recibidas**: crear unidades solo tras Todo correcto y la clasificación Stock para venta
- Habla con **Anuncios y ventas**: relacionar una unidad con anuncios, reservas y ventas mediante la referencia pública
- Habla con **Telegram**: recoger confirmaciones y correcciones de Víctor sin acciones automáticas en plataformas

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Usar stock negativo, referencias ficticias o FIFO automático para cerrar una venta.
- Crear stock desde un tracking entregado, un paquete No OK o una compra personal.
- Tratar la referencia como privada u ocultarla del título del anuncio.
- Dar por resuelta la identidad física suponiendo que habrá o que no habrá etiquetas; la solución permanece abierta. Registrar ubicaciones exactas sí queda fuera de la primera versión.
- Afirmar una correspondencia física exacta entre unidades idénticas mientras siga sin resolverse.
- Aplicar automáticamente devoluciones, sustituciones, reembolsos o fórmulas de pricing.
- Borrar o sobrescribir silenciosamente movimientos anteriores.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T04-Q01 / E-LIVE-005] ¿Cuál fue el último ajuste real de stock, con antes, después, motivo y efecto económico?
- [T04-Q02 / T04-Q05 / X-LIVE-004] ¿Cómo se distingue físicamente una unidad concreta entre ejemplares idénticos sin etiquetas?
- [T04-Q03] ¿Qué cambios pertenecen al stock, cuáles a un ajuste y cuáles a movimientos económicos?
- [T04-Q04] ¿Qué regla resuelve dos correos casi simultáneos sobre la última unidad?
- [T04-Q06 / X-LIVE-011] ¿Cómo se generan y secuencian las tareas para varias unidades iguales?
- [T03-Q08 / G-LIVE-006] ¿Qué alfabeto y política de colisiones, reutilización y agotamiento tendrá la referencia de tres caracteres?
- [T03-Q04] ¿Cómo se revierte una entrada incorrecta y sus efectos derivados sin perder la historia?
- [D-LIVE-023 / G-LIVE-017] ¿Cambiar la clasificación aprendida afecta solo a compras futuras o también a pedidos existentes?

