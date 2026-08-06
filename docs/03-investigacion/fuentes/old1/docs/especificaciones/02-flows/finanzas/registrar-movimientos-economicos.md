# Spec: Registrar costes, ingresos y gastos

Proyecto `sania-registrar-movimientos-economicos`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

## 1. Propósito

SANIA registrará únicamente importes respaldados por su fuente y distinguirá el precio de venta provisional, la transferencia final de plataforma y otros tipos de movimiento. El precio comunicado en una venta no equivale por sí solo a dinero disponible ni a beneficio definitivo.

Cuando un correo o una fuente económica comunicó un importe, Víctor necesitó conservar qué significaba, de dónde procedía y a qué operación pertenecía sin confundir precio, transferencia, gasto, reembolso o beneficio.

Criterios de éxito:
- Cada movimiento conservó tipo, importe, fecha, origen, operación relacionada y estado de evidencia.
- El precio del correo inicial se registró como importe de venta provisional, no como cierre ni beneficio.
- El cierre Vinted no duplicó la transferencia al reprocesar TX-COMPLETE.
- Los movimientos de Wallapop no se infirieron mientras el monedero no tenga una fuente definida.

## 2. Actores y vocabulario

- **Víctor**: consultó manualmente el monedero; el flujo para aportar o corregir importes, su evidencia y una posible segunda confirmación siguen pendientes

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### SANIA guardó el importe inicial sin adelantar el cierre [con la app]

- [tercero externo] Wallapop o Vinted comunicó en un correo de venta el precio del artículo o de la operación.
- [automático: código] SANIA relacionó el correo con la venta y guardó el importe y su fuente como precio de venta provisional.
- [automático: código] SANIA no lo trató como saldo disponible, movimiento final ni beneficio definitivo.

### SANIA registró los importes finales comunicados por Vinted [con la app]

- [tercero externo] Vinted envió TX-COMPLETE con número de transacción, fecha, precio del artículo, precio del envío y transferencia al saldo.
- ⚑ Regla: ¿El número de transacción ya había producido estos movimientos?
    - si sí:
        - [automático: código] SANIA conservó los movimientos existentes sin duplicarlos.
        - aquí termina este camino
    - camino normal: no, SANIA registró los importes con su significado
- [automático: código] El movimiento finalizó el pendiente de plataforma de Vinted, pero no volvió definitivo el beneficio si faltaban costes.

### SANIA no inventó el movimiento final de Wallapop [con la app]

- [automático: código] El correo de entrega de Wallapop dejó el importe pendiente del OK del comprador y no creó una entrada definitiva.
- [automático: código] SANIA mantuvo el movimiento final bloqueado porque el monedero distingue ventas, retiradas, recargas y reembolsos, pero su fuente de lectura no está definida.
- [automático: código] Un dato manual, desconocido o contradictorio quedó sin aplicar hasta que existieran evidencia y una regla; T02-Q08 dejó pendiente si además debía abrir un ticket.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- Los tres cierres Vinted mostraron por separado precio del artículo, precio del envío y transferencia al saldo; dos ventas con el mismo título tuvieron importes diferentes. [E-LIVE-007, T06-Q01, T06-Q07]
- La captura de Wallapop mostró una venta de 50,00 €, una retirada de -105,00 €, una recarga de 25,00 €, un reembolso de 80,00 € y otro movimiento de -80,00 €; no todos los importes del monedero son ventas. [E-LIVE-009, T06-Q02, X-LIVE-005]

## 5. Reglas de negocio

### D-LIVE-030: El importe de venta se registra antes de calcular beneficio

El precio comunicado se guarda con su fuente, pero no se considera beneficio definitivo hasta disponer de los costes reales completos.

### G-LIVE-001: Un hecho económico se aplica una sola vez

El mismo correo o número de transacción no crea dos movimientos, transferencias ni cierres.

### G-LIVE-011: TX-COMPLETE respalda la transferencia Vinted

El cierre conserva el número de transacción, la fecha y cada importe con su significado, sin sumar el precio del envío a la transferencia salvo que la fuente lo indique.

### X-LIVE-005: La fuente final de Wallapop sigue bloqueada

No existe correo final y todavía no se ha decidido cómo recibirá SANIA los movimientos del monedero.

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| movimiento económico | operación y plataforma relacionadas, tipo: precio comunicado, transferencia, venta, retirada, recarga, reembolso, gasto o corrección, importe y moneda, fecha, origen y evidencia, identificador externo cuando exista, estado provisional, observado o pendiente de conciliación, historial de correcciones, clasificación de sensibilidad, visibilidad y retención pendientes de T10-Q03 | correos transaccionales reconocidos y futuras fuentes económicas expresamente autorizadas |

- Habla con **Gmail**: obtener precios de venta y los importes del cierre Vinted
- Habla con **Telegram**: avisar de tickets o pedir datos cuando exista un flujo confirmado; no presupone una segunda confirmación
- Habla con **monedero de Wallapop**: fuente pendiente de autorización o mecanismo de entrada
- Habla con **Excel costes_aliexpress.xlsx**: servir como referencia actual de costes; su importación, estructura y validación siguen pendientes y no autorizan fórmulas no acordadas

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Inferir la entrada final de Wallapop desde el correo de venta o de entrega.
- Sumar indiscriminadamente precio del artículo, envío y transferencia como si fueran el mismo ingreso.
- Convertir un precio de venta en beneficio definitivo.
- Aplicar una segunda confirmación económica que todavía no se ha definido.
- Fijar en el MVP márgenes, precios mínimos o fórmulas de pricing pendientes.
- Borrar el valor anterior al corregir un movimiento.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T06-Q05] ¿Qué comisiones, portes, embalajes, impuestos y otros gastos deben registrarse y de qué fuente proceden?
- [T06-Q06] ¿Cómo se reparte un gasto que afecta a varias unidades o ventas?
- [T06-Q07] ¿Qué evidencia respalda cada importe, cómo se importará y validará la referencia actual Excel costes_aliexpress.xlsx y qué campos contiene?
- [T06-Q08, T07-Q03] ¿Qué cambios económicos requieren segunda confirmación y quién la realiza?
- [T06-Q09] ¿Cómo se corrige un movimiento de forma auditable y qué motivo es obligatorio?
- [T06-Q02, X-LIVE-005] ¿Cómo recibirá SANIA los distintos tipos de movimiento del monedero Wallapop?
- [T10-Q03] ¿Qué importes, costes y evidencias son datos delicados, quién puede verlos y cuánto tiempo se conservan?

