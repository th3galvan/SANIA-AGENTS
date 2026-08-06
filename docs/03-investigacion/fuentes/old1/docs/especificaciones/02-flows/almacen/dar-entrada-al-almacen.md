# Spec: Dar entrada a las unidades recibidas

Proyecto `sania-dar-entrada-al-almacen`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

## 1. Propósito

Esta actividad crea inventario únicamente después de la comprobación física Todo correcto y solo para productos clasificados como Stock para venta. Cada unidad creada recibe una referencia pública alfanumérica de tres caracteres; la entrada no asigna ubicaciones y lanza inmediatamente las tareas de anuncio. Pricing detallado, disputas, sustituciones y reembolsos permanecen como evolución o hipótesis.

Cuando Víctor tuvo un paquete en las manos, comprobó productos y cantidades y pulsó Todo correcto, necesitó que SANIA creara exactamente las unidades de venta confirmadas, conservara su origen y preparara su publicación sin convertir el tracking en stock.

Criterios de éxito:
- Ninguna unidad existió por un correo o tracking entregado sin Todo correcto.
- Cada unidad de Stock para venta físicamente confirmada recibió una referencia alfanumérica única de tres caracteres destinada a aparecer al final del título del anuncio.
- Las compras personales no entraron en inventario de venta ni generaron tareas de anuncio.
- Cada corrección conservó antes, después, actor, fecha y hora y motivo sin borrar el hecho original.

## 2. Actores y vocabulario

- **Víctor**: comprobó físicamente el paquete, clasificó productos cuando fue necesario y corrigió de forma explícita los datos erróneos

- "Todo correcto": confirmación de Víctor de que tuvo los productos en las manos y que productos y cantidades concordaron; es el único disparador confirmado para crear unidades
- "Stock para venta": clasificación de producto que permite crear unidades de inventario y tareas de anuncio
- "Compra personal": clasificación de producto que excluye la compra del inventario y de las tareas de anuncio
- "unidad física": ejemplar concreto creado en inventario después de Todo correcto; un anuncio representa una sola unidad
- "referencia de unidad": identificador alfanumérico único de tres caracteres por unidad, visible públicamente como sufijo del título del anuncio; alfabeto, colisiones, reutilización y agotamiento están pendientes
- "identidad física de unidades idénticas": problema abierto: unidades visualmente idénticas, sin etiqueta ni rasgo diferenciador, no permiten demostrar todavía qué ejemplar material corresponde a cada referencia lógica
- "corrección auditable": nuevo evento que conserva valor anterior, valor nuevo, actor, fecha y hora y motivo sin sobrescribir el historial
- "pricing": evolución futura basada conceptualmente en coste, margen mínimo y margen o precio de publicación; no existe fórmula exacta, redondeo ni valor fijo aprobados para el MVP

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### SANIA determinó si el producto era para venta o uso personal [con la app]

- ⚑ Regla: ¿SANIA tenía una clasificación recordada para ese producto?
    - si no, era la primera vez:
        - [persona] Víctor eligió Stock para venta o Compra personal. · Víctor
        - [automático: código] SANIA recordó la elección por producto para compras futuras.
        - …y vuelve al flujo
    - camino normal: sí, aplicó automáticamente Stock para venta o Compra personal
- [automático: código] Víctor pudo cambiar manualmente la clasificación aprendida. SANIA registró el antes, el después, el actor, la fecha y hora y el motivo; el efecto sobre pedidos ya existentes o mixtos sigue pendiente.

### SANIA procesó un paquete físicamente correcto [con la app]

- [automático: código] La actividad recibió un paquete para el que Víctor ya había pulsado Todo correcto después de contar e identificar productos y cantidades.
- ⚑ Regla: ¿El producto estaba clasificado como Stock para venta?
    - si no, era Compra personal:
        - [automático: código] SANIA registró la compra como personal sin crear unidades de inventario ni tareas de anuncio.
        - aquí termina este camino
    - camino normal: sí, continuó hacia inventario
- [automático: código] SANIA creó una unidad por cada ejemplar físicamente confirmado y vinculó cada unidad con el pedido, el paquete, el producto y la variante que pudieron demostrarse.
- [automático: código] SANIA asignó a cada unidad una referencia alfanumérica única de tres caracteres destinada a ser visible al final del título del anuncio; la política concreta de generación y colisiones quedó pendiente.
- [automático: código] SANIA dejó las unidades disponibles sin registrar estantería, caja ni posición exacta.
- [automático: código] SANIA conservó la evidencia de coste disponible, pero no aplicó una fórmula de reparto, margen o redondeo que no hubiera sido definida.
- [automático: código] Inmediatamente después de la entrada, SANIA inició la publicación asistida. En entradas grandes presentó primero las dos acciones de plataforma de un objetivo producto/unidad y después las dos del siguiente; cada acción confirmable se resolvió a una unidad y plataforma, pero X-LIVE-011 mantiene pendiente la granularidad de generación por producto o por unidad.

### SANIA bloqueó una entrada no confirmada [con la app]

- ⚑ Regla: ¿Faltó Todo correcto o Víctor pulsó No OK o Abrir disputa?
    - si no, existía Todo correcto:
        - [automático: código] SANIA continuó por el flujo normal y comprobó idempotencia antes de crear cada unidad.
        - …y vuelve al flujo
    - camino normal: sí, SANIA no creó unidades ni referencias
- [automático: código] La incidencia quedó separada del stock. No se dio por decidido si Víctor abrirá o gestionará una disputa en AliExpress ni cuál será su flujo: A-018 fue hipotético y A-048 no recuperó el detalle; resolución, recordatorios, reembolsos y sustituciones siguen pendientes.

### Víctor corrigió la clasificación aprendida o un dato de stock sin perder la historia [con la app]

- [persona] Víctor solicitó corregir la clasificación aprendida para compras futuras o ajustar una cantidad o estado de stock detectados mediante recuento, y explicó el motivo. · Víctor
- [automático: código] SANIA añadió un evento con fecha y hora, actor, origen manual, campo, valor anterior, valor nuevo y motivo sin borrar ni sobrescribir el evento previo.
- [automático: código] Este flujo genérico no autorizó corregir costes, dinero ni otros campos sensibles; su procedimiento y una posible segunda confirmación siguen pendientes en T06-Q08 y T06-Q09.
- [automático: código] El procedimiento concreto para deshacer una confirmación física y revertir unidades o tareas ya creadas sigue pendiente.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

### REC-1: Crear unidades de venta después de la comprobación física (pendiente · 1ª entrega)

Convertir únicamente contenido físicamente correcto y clasificado para venta en unidades trazables y tareas de publicación.

- **R-1**: Reprocesar el mismo correo o la misma confirmación no crea paquetes, unidades ni tareas duplicadas.
- **R-2**: Sin Todo correcto no existe ninguna unidad de inventario derivada del paquete.
- **R-3**: Todo correcto crea una unidad por ejemplar de Stock para venta confirmado y lanza inmediatamente las tareas de anuncio.
- **R-4**: Cada unidad creada recibe una referencia pública alfanumérica única de tres caracteres y no una referencia privada.

- **C-1**: Dado un tracking entregado sin respuesta de Víctor / Cuando SANIA procesó el evento / Entonces el paquete quedó pendiente de comprobación y no se creó ninguna unidad
- **C-2**: Dado un paquete con N ejemplares de Stock para venta comprobados físicamente / Cuando Víctor pulsó Todo correcto / Entonces SANIA creó N unidades una sola vez, con N referencias de tres caracteres, y lanzó sus tareas de publicación
- **C-3**: Dado un paquete correcto clasificado como Compra personal / Cuando Víctor pulsó Todo correcto / Entonces SANIA no creó inventario de venta ni tareas de anuncio

### REC-2: Corregir una entrada o clasificación (pendiente)

Rectificar datos sin perder el hecho original ni ocultar quién cambió qué.

- **R-5**: Toda corrección añade valor anterior, valor nuevo, actor, fecha y hora y motivo.
- **R-6**: La clasificación aprendida por producto puede corregirse manualmente.

- **C-4**: Dado una regla de clasificación aprendida o una cantidad de stock registrada de forma equivocada / Cuando Víctor la corrigió y explicó el motivo / Entonces SANIA conservó el evento original y añadió el cambio con antes, después, actor, fecha y motivo

### Episodios reales que sustentan los requisitos

- Un pedido AliExpress puede dividirse, consolidarse y llegar a cuentagotas. Solo los productos y cantidades que Víctor tiene en las manos y confirma físicamente pueden continuar hacia la entrada. [T02-Q03, T02-Q09, T03-Q01, D-LIVE-017, G-LIVE-002]
- Hoy, cuando Víctor detecta un fallo de stock, cambia manualmente el Excel. SANIA debe sustituir ese gesto por una corrección con antes, después, actor, fecha y motivo. [T03-Q04, T04-Q01]
- El plano v2 documentó el pedido 3074442296049454, variante BLACK, cantidad 7 y total 80,72 €, además de un cálculo de 15,95 € basado en 25 % y terminación ,95. LIVE solo confirma el concepto futuro de coste y dos márgenes; no confirma esa fórmula ni ese redondeo como regla general. [D-LIVE-026, G-LIVE-019, T03-Q05, T03-Q06, X-LIVE-009]
- El plano v2 recogió una fila de Excel «x2 9 mm» con precio 7,00 €, coste 6,76 € y una fórmula de margen. Se conserva como ejemplo histórico del plano anterior, no como fórmula aprobada por LIVE. [D-LIVE-026, T03-Q06, X-LIVE-009]

## 5. Reglas de negocio

### G-LIVE-001: Idempotencia

Un hecho externo o una confirmación se aplica una sola vez; no crea unidades ni tareas duplicadas.

### G-LIVE-002: Tracking entregado no crea stock

El tracking entregado deja el paquete pendiente de comprobación física.

### G-LIVE-003: Todo correcto crea unidades y tareas

Todo correcto significa producto y cantidades comprobados. Para Stock para venta, SANIA registra las unidades y lanza inmediatamente las tareas de anuncio.

### G-LIVE-004: No OK bloquea la entrada

No OK o Abrir disputa crea una incidencia y no crea ni inventa unidades.

### G-LIVE-006: Referencia pública de tres caracteres

Cada unidad creada recibe una referencia alfanumérica única de tres caracteres visible al final del título del anuncio.

### G-LIVE-016: Compra personal fuera del inventario

Solo Stock para venta entra en inventario y genera anuncios.

### G-LIVE-017: Clasificación recordada y corregible

SANIA recuerda por producto Stock para venta o Compra personal y Víctor puede cambiar la elección manualmente.

### G-LIVE-018: Sin ubicaciones

La primera versión no registra estantería, caja ni posición física exacta.

### D-LIVE-028: Sin ubicación detallada en la primera versión

Tras Todo correcto se crea stock y se pasa a publicación sin introducir estantería, caja ni posición física exacta.

### CORRECCION-AUDITABLE: Correcciones sin sobrescritura

Cada corrección conserva valor anterior, valor nuevo, actor, fecha y hora y motivo; el flujo concreto de deshacer una confirmación está pendiente.

## 6. Estados

### clasificación aprendida del producto para compras futuras

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| sin regla aprendida | Víctor eligió Stock para venta para futuras compras del producto (Víctor) → pasa a 'Stock para venta' · Víctor eligió Compra personal para futuras compras del producto (Víctor) → pasa a 'Compra personal' |
| Stock para venta | Víctor actualizó la regla para futuras compras (Víctor) → pasa a 'Compra personal' |
| Compra personal | Víctor actualizó la regla para futuras compras (Víctor) → pasa a 'Stock para venta' |

### paquete

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| entregado pendiente de comprobación | Víctor pulsó Todo correcto (Víctor) → pasa a 'recibido correcto' · Víctor pulsó No OK o Abrir disputa (Víctor) → pasa a 'incidencia' |
| recibido correcto |  |
| incidencia |  |

### unidad de inventario

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| disponible |  |

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| clasificación del producto | producto, Stock para venta o Compra personal, origen manual o aplicación de regla aprendida, valor anterior y nuevo en correcciones, actor, fecha y motivo | respuesta o corrección de Víctor |
| unidad | referencia alfanumérica pública de tres caracteres, producto y variante confirmados, pedido, paquete y línea cuando sean identificables, estado de disponibilidad, evidencia de coste sin fórmula de reparto asumida, tareas de anuncio relacionadas, historial | Todo correcto sobre contenido clasificado como Stock para venta |
| evento del historial | entidad afectada, fecha y hora, actor, origen automático o manual, campo, valor anterior, valor nuevo, motivo | cada creación o corrección realizada por SANIA o Víctor |

- Habla con **Confirmar la recepción de pedidos**: recibir exclusivamente paquetes con Todo correcto o mantener bloqueadas sus incidencias
- Habla con **Telegram**: clasificar productos, confirmar acciones y comunicar tareas de publicación; sin cadencias inventadas
- Habla con **Creación asistida de anuncios**: lanzar tareas inmediatamente después de crear unidades de Stock para venta
- Habla con **Excel de costes**: conservarlo como fuente histórica actual, no como autorización de fórmulas de pricing no confirmadas

## 8. Superficie de uso

### Inventario de Víctor

| Campo | Valor |
|---|---|
| Quién entra | Víctor |
| Por dónde llega |  |
| Cuándo lo usa | quiso revisar una entrada, unidad, clasificación o corrección |
| Qué ve nada más entrar | origen del pedido y paquete, producto, variante, referencia pública, estado, evidencia de coste e historial |
| Qué puede hacer | cambiar la clasificación aprendida · consultar unidades y tareas derivadas · solicitar una corrección explicando el motivo |
| Qué NO debe poder jamás | crear una unidad por el tracking · asignar una ubicación detallada · borrar el historial · aplicar automáticamente una fórmula de pricing, disputa, sustitución o reembolso no confirmada |

## 9. Calidad y límites

- **Q-1**: Cero unidades creadas desde un tracking entregado sin Todo correcto.
- **Q-2**: Para N ejemplares físicamente confirmados y clasificados como Stock para venta existen exactamente N unidades y N referencias alfanuméricas de tres caracteres, incluso tras reprocesar el hecho.
- **Q-3**: Cero compras personales incorporadas al inventario de venta o a tareas de anuncio.
- **Q-4**: El 100 % de las correcciones admitidas por este flujo conserva antes, después, actor, fecha y hora y motivo.
- **Q-5**: Cero ubicaciones físicas detalladas registradas en la primera versión.

## 10. Fuera de alcance

- Crear unidades antes de Todo correcto o para Compra personal.
- Crear referencias para unidades ausentes, defectuosas o no comprobadas.
- Dar por resuelta la identidad física suponiendo que habrá o que no habrá etiquetas; la solución permanece abierta. Registrar ubicaciones físicas detalladas sí queda fuera de la primera versión.
- Resolver automáticamente la identidad exacta entre unidades físicamente idénticas.
- Aplicar un margen fijo del 25 %, un redondeo a ,95 o cualquier fórmula exacta de pricing en el MVP.
- Abrir, negociar o cerrar automáticamente disputas en AliExpress.
- Aplicar reglas operativas de reembolso, sustitución, devolución o traslado de costes sin un caso real y una decisión posterior.
- Fijar recordatorios semanales u otra cadencia no confirmada.
- Automatizar negociaciones de precio con compradores de Wallapop o Vinted.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T02-Q02] ¿Qué identificador estable de AliExpress distingue una línea y cómo se relaciona con paquetes divididos o consolidados?
- [T03-Q01 / T03-Q09] ¿Cuál es el flujo real cuando el primer paquete llegue incompleto, defectuoso o con variante errónea, y qué resultado económico produce?
- [T03-Q04] ¿Cómo se deshace Todo correcto si ya creó unidades y tareas de anuncio?
- [T03-Q05] ¿Con qué precisión se guarda y muestra el coste unitario?
- [T03-Q06 / D-LIVE-026] ¿Cuál será la fórmula de pricing, qué costes incluye y difiere por plataforma?
- [T03-Q07] ¿Debe existir un recordatorio de disputa y, en su caso, con qué cadencia confirmada?
- [T03-Q08 / G-LIVE-006] ¿Qué alfabeto, política de colisiones, reutilización y agotamiento usa la referencia de tres caracteres?
- [T04-Q02 / T04-Q05 / X-LIVE-004] ¿Cómo se mantiene identidad exacta entre referencia lógica y unidades físicamente idénticas sin etiquetas?
- [T04-Q06 / X-LIVE-011] ¿Las tareas para varias unidades iguales se secuencian por unidad o de otro modo?
- [D-LIVE-023 / G-LIVE-017] ¿Qué efecto tiene corregir la clasificación aprendida sobre pedidos ya existentes o mixtos?

