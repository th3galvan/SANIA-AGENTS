# Spec: Seguir pedidos de AliExpress

Proyecto `sania-seguir-pedidos-aliexpress`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

## 1. Propósito

Esta actividad convertirá en pedidos, líneas, paquetes y eventos trazables únicamente los correos de AliExpress cuyas plantillas reales se aporten y validen. LIVE no incluyó correos de AliExpress. La división, consolidación y recepción parcial sí se confirmaron como comportamiento operativo observado, pero todavía no como campos de una plantilla; ningún estado logístico crea stock por sí solo.

Cuando AliExpress comunicó una compra o un cambio de seguimiento, Víctor necesitó que SANIA actualizara una sola vez el pedido y el paquete correctos, mantuviera visibles las partes pendientes y separara la entrega logística de la comprobación física.

## 2. Actores y vocabulario

- **Víctor**: consultó pedidos, clasificó productos cuando fue necesario y resolvió los datos que SANIA no pudo identificar con seguridad

- "pedido": compra de AliExpress que puede contener varias líneas y permanecer abierta mientras existan paquetes pendientes de comprobación física
- "línea de compra": parte del pedido asociada a un producto o variante; su identificador estable todavía no se ha confirmado
- "paquete": unidad logística con seguimiento propio cuando esté disponible; puede contener partes de una o varias líneas por división o consolidación
- "entregado": estado logístico que abre una comprobación física y nunca equivale por sí solo a recibido correcto ni a stock disponible

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### SANIA actualizó un pedido desde un correo reconocido [con la app]

- [tercero externo] SANIA recibió un correo de AliExpress perteneciente a una plantilla real previamente aportada y validada. LIVE no aportó ninguna plantilla que demuestre los tipos o campos exactos de compra, envío, división, consolidación, seguimiento o entrega.
- ⚠ Excepción: ¿El remitente, el tipo de correo y sus campos coincidieron con un patrón probado?
    - si no, el formato fue desconocido, ambiguo o contradictorio:
        - [automático: código] SANIA conservó el correo, dejó sus datos sin aplicar y abrió una revisión; el uso de IA para proponer extracciones sigue pendiente de límites y aprobación humana.
        - aquí termina este camino
    - camino normal: sí, SANIA extrajo únicamente los datos sustentados por el patrón
- [automático: código] SANIA conservó el evento original y comprobó su idempotencia antes de modificar el pedido.
- ⚠ Excepción: ¿Los identificadores disponibles permitieron relacionar con seguridad pedido, línea y paquete?
    - si no, faltó un identificador estable o una relación:
        - [automático: código] SANIA dejó la relación pendiente y pidió a Víctor la comprobación necesaria sin inventar pedido, línea, producto, variante ni paquete.
        - …y vuelve al flujo
    - camino normal: sí, SANIA mantuvo las relaciones observadas
- [automático: código] SANIA representó por separado cada paquete observado y permitió que un pedido tuviera paquetes divididos, consolidados o recibidos a cuentagotas.
- ⚑ Regla: ¿El evento indicó que un paquete fue entregado?
    - si sí:
        - [automático: código] SANIA cambió el paquete a pendiente de comprobación física y creó la tarea de recepción; no creó unidades ni aumentó el stock.
        - …y vuelve al flujo
    - camino normal: no, SANIA actualizó únicamente el seguimiento
- [automático: código] El pedido permaneció abierto mientras existieran paquetes pendientes, aunque las unidades de un paquete ya comprobado pudieran continuar su propio flujo.

### SANIA aplicó la clasificación del producto comprado [con la app]

- ⚑ Regla: ¿Existía una clasificación recordada para ese producto?
    - si no, era la primera vez:
        - [persona] Víctor eligió Stock para venta o Compra personal. · Víctor
        - [automático: código] SANIA recordó la elección para compras futuras del mismo producto.
        - …y vuelve al flujo
    - camino normal: sí, SANIA aplicó Stock para venta o Compra personal
- [automático: código] Víctor pudo cambiar manualmente la clasificación aprendida; SANIA conservó el cambio de forma auditable. El efecto sobre pedidos ya existentes y los pedidos mixtos sigue pendiente.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- Un pedido de AliExpress con productos de distintos vendedores puede llegar separado, consolidarse en un centro logístico y recibirse a cuentagotas. Solo lo físicamente comprobado continúa hacia stock y el pedido parcial permanece abierto. [T02-Q03, T02-Q09, T03-Q01, D-LIVE-017, G-LIVE-002]

## 5. Reglas de negocio

### G-LIVE-001: Idempotencia de hechos externos

Un correo o hecho externo reconocido se aplica una sola vez; no se duplican pedidos, paquetes, líneas ni cambios de estado.

### G-LIVE-002: Entrega logística separada del stock

El tracking entregado solo deja el paquete pendiente de comprobación física; nunca crea stock.

### G-LIVE-016: Compra personal fuera del stock de venta

Solo los productos clasificados como Stock para venta pueden continuar hacia inventario y tareas de anuncio.

### G-LIVE-017: Clasificación aprendida corregible

La clasificación se recuerda por producto y Víctor puede cambiarla manualmente.

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| pedido de AliExpress | identificador externo cuando sea observable, líneas y relaciones pendientes, paquetes divididos o consolidados, estado operativo observado y partes pendientes, con catálogo final de datos y estados todavía por definir, eventos originales, correos de origen | futuro corpus real de correos de AliExpress, todavía no aportado en LIVE |
| paquete | pedido relacionado, tracking cuando exista, líneas o partes relacionadas cuando puedan demostrarse, estado logístico, estado de comprobación física, correo de origen | futuras plantillas y eventos reales de seguimiento, todavía no aportados en LIVE |
| clasificación del producto | Stock para venta o Compra personal, producto al que aplica, origen manual o aprendido, valor anterior y nuevo, actor, fecha y motivo del cambio | respuesta o corrección de Víctor |

- Habla con **Gmail de solo lectura**: recibir y conservar los correos de AliExpress sin ejecutar acciones en la plataforma
- Habla con **Telegram**: pedir a Víctor clasificaciones, relaciones o comprobaciones que faltaron
- Habla con **Confirmar la recepción de pedidos**: abrir una comprobación física cuando el seguimiento indique entrega

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Realizar la compra o actuar dentro de AliExpress.
- Inventar un producto, variante, identificador o relación entre línea y paquete.
- Tratar un estado de tracking como confirmación física o como entrada de stock.
- Cerrar un pedido parcial mientras queden paquetes sin comprobar.
- Fijar una cadencia de recordatorios no confirmada.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T02-Q02] ¿Qué identificador estable distingue una línea de AliExpress cuando cambia el texto visible?
- [T02-Q03] ¿Qué plantillas reales demuestran compra, envío, división, consolidación y entrega, y cómo expresan las relaciones entre pedido, línea y paquete?
- [T02-Q05] ¿Qué ocurrió la última vez que un correo llegó repetido, tarde o fuera de orden?
- [T02-Q06] ¿Qué patrones, remitentes y campos son obligatorios en cada variante real de correo de AliExpress?
- [T02-Q07] ¿Cuándo puede una IA proponer una extracción y qué revisión humana exige?
- [T02-Q08] ¿Cómo se clasifica y revisa un correo desconocido, dudoso o contradictorio sin aplicar datos al pedido o al stock?
- [T02-Q09] ¿Qué datos y estados debe mostrar SANIA mientras un pedido esté parcialmente recibido?
- [D-LIVE-023 / G-LIVE-017] ¿Qué efecto tiene cambiar una clasificación aprendida sobre pedidos ya existentes o mixtos?

