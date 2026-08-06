# Spec: Calcular el beneficio real

Proyecto `sania-calcular-beneficio-real`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

## 1. Propósito

En el MVP SANIA no presentará un beneficio como definitivo porque el informe no define todos los costes, su reparto ni la fórmula completa. Podrá mostrar que el resultado está pendiente y qué datos faltan; el cálculo definitivo queda condicionado a una venta realmente cerrada y a un modelo financiero acordado.

Cuando una plataforma terminó una venta, Víctor necesitó distinguir el importe recibido del beneficio real y ver por qué el resultado seguía provisional si faltaban costes o reglas.

Criterios de éxito:
- Ninguna venta abierta o bloqueada produjo un beneficio definitivo.
- Una venta cerrada siguió provisional cuando faltaban comisiones, portes, embalajes, impuestos, reparto o fórmula.
- SANIA mostró los datos ausentes en lugar de sustituirlos por cero o inventarlos.
- El pricing por márgenes se mantuvo en evolución y no alteró el cálculo real del MVP.

## 2. Actores y vocabulario

- **Víctor**: consultará qué datos faltan; el flujo para aportar o corregir costes y ajustes, su evidencia y una posible segunda confirmación siguen pendientes

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### SANIA determinó si el beneficio podía considerarse definitivo [con la app]

- ⚑ Regla: ¿La venta estaba cerrada por una señal final válida de su plataforma?
    - si no o Wallapop seguía bloqueada:
        - [automático: código] SANIA mantuvo el resultado como provisional y mostró que faltaba el cierre de plataforma.
        - aquí termina este camino
    - camino normal: sí, SANIA revisó los importes
- ⚠ Excepción: ¿Estaban definidos y respaldados todos los costes, su reparto y la fórmula de beneficio?
    - si no, situación actual:
        - [automático: código] SANIA mantuvo el resultado como provisional o no calculable, enumeró los datos ausentes y no los sustituyó por cero.
        - aquí termina este camino
    - camino normal: sí, el cálculo definitivo será posible cuando se acuerde el modelo
- [automático: código] En evolución, SANIA calculará y conservará el beneficio y el margen con cada importe y regla de origen una vez validados con casos reales completos.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- Los cierres Vinted aportaron precio del artículo, envío y transferencia al saldo, pero no comisiones, embalaje, impuestos ni el resto de costes necesarios para validar un beneficio real. [E-LIVE-007, T06-Q07, T06-Q10, D-LIVE-030]
- La entrega de Wallapop por 50,00 € no cerró la venta; por ello tampoco pudo convertir el ingreso provisional en beneficio definitivo. [E-LIVE-008, T06-Q02, X-LIVE-005]

## 5. Reglas de negocio

### D-LIVE-030: El precio guardado no es beneficio

El importe de venta se conserva y las finanzas lo usarán después; no se cierra el beneficio hasta disponer de costes reales.

### D-LIVE-026: Pricing por dos márgenes queda en evolución

Precio de publicación y mínimo se derivarán del coste y dos márgenes cuando se defina la fórmula; el 25 % fue un ejemplo, no un valor fijado para el MVP.

### X-LIVE-005: Wallapop bloquea su beneficio mientras no cierre

Sin una fuente final del monedero, SANIA no puede cerrar la venta ni declarar definitivo su resultado.

## 6. Estados

### resultado de venta

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| pendiente de cierre | recibir una señal final válida de plataforma → pasa a 'pendiente de costes completos' |
| pendiente de costes completos | mostrar importes y reglas ausentes sin calcular un resultado definitivo |
| definitivo | estado futuro disponible solo cuando estén acordados y validados todos los costes, reparto y fórmula |

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| resultado de venta | venta y plataforma, estado del cierre, ingresos con su fuente, costes y gastos conocidos con su fuente, datos pendientes, regla y versión de cálculo cuando exista, beneficio y margen solo si pueden calcularse, estado provisional, no calculable o definitivo, clasificación de sensibilidad, visibilidad y retención pendientes de T10-Q03 | venta, movimientos económicos relacionados y Excel costes_aliexpress.xlsx como referencia actual pendiente de importación y validación |

- Habla con **Excel costes_aliexpress.xlsx**: servir como referencia actual de costes sin asumir todavía su importación, estructura ni fórmula

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Dar por definitivo un resultado mientras la venta o una incidencia siguió abierta.
- Tratar la entrega logística de Wallapop como cierre económico.
- Inventar comisiones, portes, embalajes, impuestos, reparto o importes ausentes.
- Usar cero como valor por defecto para un coste desconocido.
- Fijar el 25 % como margen obligatorio o implementar pricing avanzado en el MVP.
- Calcular aquí negociación, contraofertas o precios dinámicos.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T06-Q02, X-LIVE-005] ¿Qué hecho observable volverá definitivo el ingreso de Wallapop?
- [T06-Q05] ¿Qué comisiones, portes, embalajes, impuestos y otros gastos se restarán y de qué fuente procederán?
- [T06-Q06] ¿Cómo se repartirá un gasto entre varias unidades o ventas?
- [T06-Q10] ¿Qué venta real normal y qué venta rara con todos los costes validarán el cálculo?
- [T03-Q05] ¿Qué precisión, redondeo y formato visible se usarán en costes y resultados?
- [T03-Q06, T09-Q06, X-LIVE-009] ¿Cuál será la fórmula futura de los dos márgenes y variará por plataforma?
- [T06-Q07] ¿Cómo se importará y validará Excel costes_aliexpress.xlsx como referencia actual de costes?
- [T10-Q03] ¿Qué costes, resultados y evidencias son datos delicados, quién puede verlos y cuánto tiempo se conservan?

