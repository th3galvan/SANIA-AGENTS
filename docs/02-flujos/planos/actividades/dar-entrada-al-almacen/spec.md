# Spec: Dar entrada a las unidades recibidas

Proyecto `sania-dar-entrada-al-almacen`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

Esta actividad crea inventario únicamente después de la comprobación física Todo correcto y solo para productos clasificados como Stock para venta. Cada unidad creada recibe una referencia pública alfanumérica que empieza con tres caracteres; la entrada no asigna ubicaciones y lanza inmediatamente las tareas de anuncio. Pricing detallado, disputas, sustituciones y reembolsos permanecen fuera de esta primera versión.

Cuando Víctor tuvo un paquete en las manos, comprobó productos y cantidades y pulsó Todo correcto, necesitó que SANIA creara exactamente las unidades de venta confirmadas, conservara su origen y preparara su publicación sin convertir el tracking en stock.

Criterios de éxito:
- Ninguna unidad existió por un correo o tracking entregado sin Todo correcto.
- Cada unidad de Stock para venta físicamente confirmada recibió una referencia alfanumérica pública única que empezó con tres caracteres y apareció al final del título del anuncio.
- Las compras personales no entraron en inventario de venta ni generaron tareas de anuncio.
- Cada corrección conservó antes, después, actor, fecha y hora y motivo sin borrar el hecho original.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

- "Todo correcto": confirmación humana de que Víctor tuvo el contenido en las manos y que productos y cantidades concordaron; no es una confirmación del tracking
- "Stock para venta": clasificación de producto que permite crear unidades de inventario y tareas de anuncio
- "Compra personal": clasificación de producto que no cuenta como stock para venta ni genera nuevas tareas de anuncio; si el pedido ya estaba confirmado, SANIA muestra qué productos se borrarán del stock y exige confirmación antes de retirarlos, conservando el cambio en el historial
- "unidad física": ejemplar concreto creado en inventario después de Todo correcto; conserva su propia referencia y puede ser la unidad asignada a un anuncio cuando SANIA lo crea
- "referencia de unidad": identificador lógico único que empieza con tres letras y también aparece como sufijo público del anuncio. Usa el alfabeto explícito Z, Y, ..., A, z, y, ..., a; comienza en ZZZ, no se reutiliza nunca y, al agotarse una longitud, añade una letra y reinicia desde el valor máximo, por ejemplo ZZZZ. Antes del etiquetado físico individual, la referencia puede satisfacerse con cualquiera de las unidades idénticas disponibles; después del etiquetado identifica una unidad física concreta
- "identidad física de unidades idénticas": problema abierto: unidades visualmente idénticas, sin etiqueta ni rasgo diferenciador, no permiten demostrar todavía qué ejemplar material corresponde a cada referencia lógica
- "corrección auditable": nuevo evento que conserva valor anterior, valor nuevo, actor, fecha y hora y motivo sin sobrescribir el historial
- "pricing": evolución futura basada conceptualmente en coste, margen mínimo y margen o precio de publicación; no existe fórmula exacta, redondeo ni valor fijo aprobados para el MVP

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### SANIA determinó si el producto era para venta o uso personal [con la app · origen: usuario]

- ⚑ Regla: ¿SANIA tenía una clasificación recordada para ese producto?
    - si no, era la primera vez:
        - [persona] Víctor eligió Stock para venta o Compra personal. · Víctor
        - [automático: código] SANIA recordó la elección por producto para compras futuras.
        - …y vuelve al flujo
    - camino normal: sí, aplicó automáticamente Stock para venta o Compra personal
- [automático: código] Víctor pudo cambiar manualmente la clasificación aprendida desde el mismo pedido. Si el pedido ya tenía Todo correcto y cambió a Compra personal, SANIA enumeró los productos afectados y pidió confirmación; solo después del sí los borró del stock activo y registró el cambio.

### SANIA procesó un paquete físicamente correcto [con la app · origen: usuario]

- [automático: código] La actividad recibió un paquete para el que Víctor ya había pulsado Todo correcto después de contar e identificar productos y cantidades.
- ⚑ Regla: ¿El producto estaba clasificado como Stock para venta?
    - si no, era Compra personal:
        - [automático: código] SANIA registró la compra como personal sin crear unidades de inventario ni tareas de anuncio.
        - aquí termina este camino
    - camino normal: sí, continuó hacia inventario
- [automático: código] SANIA creó una unidad por cada ejemplar físicamente confirmado y vinculó cada unidad con el pedido, el paquete, el producto y la variante que pudieron demostrarse.
- [automático: código] SANIA asignó a cada unidad una referencia pública única. Empezó en ZZZ, siguió la secuencia Z a A y z a a, no reutilizó valores y añadió una letra al agotar una longitud.
- [automático: código] SANIA dejó las unidades disponibles sin registrar estantería, caja ni posición exacta.
- [automático: código] SANIA conservó la evidencia de coste disponible, pero no aplicó una fórmula de reparto, margen o redondeo que no hubiera sido definida.
- [automático: código] Inmediatamente después de la entrada, SANIA inició la publicación asistida. Para varias unidades idénticas del mismo producto creó una tarea para Wallapop y otra para Vinted, vinculadas con todas esas unidades.

### SANIA bloqueó una entrada no confirmada [con la app · origen: usuario]

- ⚠ Excepción: ¿Faltó Todo correcto o Víctor pulsó No OK o Abrir disputa?
    - si no, existía Todo correcto:
        - [automático: código] SANIA continuó por el flujo normal y comprobó idempotencia antes de crear cada unidad.
        - …y vuelve al flujo
    - camino normal: sí, SANIA no creó unidades ni referencias
- [automático: código] La incidencia quedó separada del stock. No se dio por decidido si Víctor abrirá o gestionará una disputa en AliExpress ni cuál será su flujo: A-018 fue hipotético y A-048 no recuperó el detalle; resolución, recordatorios, reembolsos y sustituciones siguen pendientes.

### Víctor cambió un pedido confirmado a Compra personal y retiró sus productos del stock [con la app · origen: usuario]

- [persona] Desde el mismo pedido que aparecía como Todo correcto, Víctor cambió la clasificación a Compra personal. · Víctor
- [automático: código] SANIA mostró los diferentes productos generados que se borrarían del stock y preguntó: «¿Estás seguro de que quieres ponerlo como Compra personal?».
- [persona] Víctor confirmó que sí quería realizar el cambio. · Víctor
- [automático: código] SANIA borró del stock activo las unidades generadas por ese pedido y añadió un evento con fecha y hora, actor, valor anterior, valor nuevo y motivo, sin borrar el historial del pedido.
- [automático: código] Este flujo genérico no autorizó corregir costes, dinero ni otros campos sensibles; su procedimiento y una posible segunda confirmación siguen pendientes en T06-Q08 y T06-Q09.
- [automático: código] SANIA borró automáticamente todas las tareas de anuncio relacionadas con los productos retirados del stock.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

### REC-1: Crear unidades de venta después de la comprobación física (pendiente · 1ª entrega)

Convertir únicamente contenido físicamente correcto y clasificado para venta en unidades trazables y tareas de publicación.

- **R-1**: Reprocesar el mismo correo o la misma confirmación no crea paquetes, unidades ni tareas duplicadas. · regla G-49 · origen: usuario · código actual: no verificado
- **R-2**: Sin Todo correcto no existe ninguna unidad de inventario derivada del paquete. · regla G-50 · origen: usuario · código actual: no verificado
- **R-3**: Todo correcto crea una unidad por ejemplar de Stock para venta confirmado y lanza inmediatamente las tareas de anuncio. · regla G-51 · origen: usuario · código actual: no verificado
- **R-4**: Cada unidad creada recibe una referencia pública única que empieza con tres caracteres, sigue la secuencia acordada, no se reutiliza y aumenta de longitud cuando se agota. · regla G-53 · origen: usuario · código actual: no verificado
- **R-7**: Todo correcto sobre una Compra personal no crea inventario de venta ni tareas de anuncio. · regla G-54 · origen: usuario · código actual: no verificado
- **R-8**: No OK o Abrir disputa crea una incidencia separada y no crea unidades ni referencias. · regla G-52 · origen: usuario · código actual: no verificado
- **R-9**: La primera versión deja cada unidad disponible sin registrar estantería, caja ni posición física exacta. · regla G-56 · origen: usuario · código actual: no verificado

- **C-1**: Dado un tracking entregado sin respuesta de Víctor / Cuando SANIA procesó el evento / Entonces el paquete quedó pendiente de comprobación y no se creó ninguna unidad · cubre R-2
- **C-2**: Dado un paquete con N ejemplares de Stock para venta comprobados físicamente / Cuando Víctor pulsó Todo correcto / Entonces SANIA creó N unidades una sola vez, con N referencias públicas únicas, y lanzó sus tareas de publicación · cubre R-1
- **C-3**: Dado un paquete con N ejemplares de Stock para venta comprobados físicamente / Cuando Víctor pulsó Todo correcto / Entonces SANIA creó N unidades una sola vez, con N referencias públicas únicas, y lanzó sus tareas de publicación · cubre R-3
- **C-4**: Dado un paquete con N ejemplares de Stock para venta comprobados físicamente / Cuando Víctor pulsó Todo correcto / Entonces SANIA creó N unidades una sola vez; las referencias empezaron en ZZZ, siguieron el orden acordado, no se reutilizaron y aumentaron de longitud si se agotó el espacio disponible · cubre R-4
- **C-5**: Dado un paquete correcto clasificado como Compra personal / Cuando Víctor pulsó Todo correcto / Entonces SANIA no creó inventario de venta ni tareas de anuncio · cubre R-7
- **C-8**: Dado un paquete incompleto, defectuoso o no confirmado / Cuando Víctor pulsó No OK o Abrir disputa / Entonces SANIA creó una incidencia separada y no creó unidades ni referencias · cubre R-8
- **C-9**: Dado una unidad creada después de Todo correcto / Cuando SANIA terminó la entrada de almacén / Entonces la unidad quedó disponible sin estantería, caja ni posición física exacta · cubre R-9

### REC-2: Corregir una entrada o clasificación (pendiente)

Rectificar datos sin perder el hecho original ni ocultar quién cambió qué.

- **R-5**: Toda corrección añade valor anterior, valor nuevo, actor, fecha y hora y motivo. · regla G-58 · origen: usuario · código actual: no verificado
- **R-6**: La clasificación aprendida puede cambiarse desde el mismo pedido; si pasa a Compra personal después de Todo correcto, SANIA muestra los productos afectados, pide confirmación y, solo tras el sí, los borra del stock activo junto con las tareas de anuncio relacionadas. · regla G-55 · origen: usuario · código actual: no verificado

- **C-6**: Dado una regla de clasificación aprendida o una cantidad de stock registrada de forma equivocada / Cuando Víctor la corrigió y explicó el motivo / Entonces SANIA conservó el evento original y añadió el cambio con antes, después, actor, fecha y motivo · cubre R-5
- **C-7**: Dado una regla de clasificación aprendida o una cantidad de stock registrada de forma equivocada / Cuando Víctor la corrigió y explicó el motivo / Entonces SANIA enumeró los productos que se borrarían del stock y pidió confirmación; tras el sí, retiró esas unidades del stock activo y borró automáticamente las tareas de anuncio relacionadas · cubre R-6

### Episodios reales que sustentan los requisitos

- Un pedido AliExpress puede dividirse, consolidarse y llegar a cuentagotas. Solo los productos y cantidades que Víctor tiene en las manos y confirma físicamente pueden continuar hacia la entrada. [Migración: identificador histórico E-LIVE-004; referencias históricas: T02-Q03, T02-Q09, T03-Q01, D-LIVE-017] [G-50]
- Hoy, cuando Víctor detecta un fallo de stock, cambia manualmente el Excel. SANIA debe sustituir ese gesto por una corrección con antes, después, actor, fecha y motivo. [Migración: identificador histórico E-LIVE-005; referencias históricas: T03-Q04, T04-Q01]
- El plano v2 documentó el pedido 3074442296049454, variante BLACK, cantidad 7 y total 80,72 €, además de un cálculo de 15,95 € basado en 25 % y terminación ,95. LIVE solo confirma el concepto futuro de coste y dos márgenes; no confirma esa fórmula ni ese redondeo como regla general. [Migración: identificador histórico ANTERIOR-V2-001; estado histórico: antecedente conservado; no define una regla vigente; referencias históricas: D-LIVE-026, G-LIVE-019, T03-Q05, T03-Q06, X-LIVE-009]
- El plano v2 recogió una fila de Excel «x2 9 mm» con precio 7,00 €, coste 6,76 € y una fórmula de margen. Se conserva como ejemplo histórico del plano anterior, no como fórmula aprobada por LIVE. [Migración: identificador histórico ANTERIOR-V2-002; estado histórico: antecedente conservado; no define una regla vigente; referencias históricas: D-LIVE-026, T03-Q06, X-LIVE-009]

## 5. Reglas de negocio

### G-49: Idempotencia

Un hecho externo o una confirmación se aplica una sola vez; no crea unidades ni tareas duplicadas. [Migración: identificador histórico G-LIVE-001; estado histórico: protección conservada; faltan casos reales de duplicado]

### G-50: Tracking entregado no crea stock

El tracking entregado deja el paquete pendiente de comprobación física. [Migración: identificador histórico G-LIVE-002; referencias históricas: D-LIVE-017]

### G-51: Todo correcto crea unidades y tareas

Todo correcto significa producto y cantidades comprobados. Para Stock para venta, SANIA registra las unidades y lanza inmediatamente las tareas de anuncio. [Migración: identificador histórico G-LIVE-003; referencias históricas: D-LIVE-017, D-LIVE-020]

### G-52: No OK bloquea la entrada

No OK o Abrir disputa crea una incidencia y no crea ni inventa unidades. [Migración: identificador histórico G-LIVE-004; estado histórico: flujo interno de disputa pendiente; referencias históricas: D-LIVE-018, T03-Q01, T03-Q09]

### G-53: Referencia pública de tres caracteres

Cada unidad creada recibe una referencia pública única. La secuencia usa Z, Y, ..., A, z, y, ..., a; empieza en ZZZ, nunca reutiliza un valor y, al agotar una longitud, añade una letra y reinicia desde el valor máximo. [Migración: identificador histórico G-LIVE-006; referencias históricas: D-LIVE-006, X-LIVE-001, T03-Q08]

### G-54: Compra personal fuera del inventario

Solo Stock para venta entra en inventario y genera anuncios. [Migración: identificador histórico G-LIVE-016; referencias históricas: D-LIVE-022]

### G-55: Clasificación recordada y corregible

SANIA recuerda por producto Stock para venta o Compra personal y Víctor puede cambiar la elección manualmente desde el pedido. Si el pedido ya tenía Todo correcto, SANIA enumera los productos que se borrarán del stock y solo los retira tras una confirmación expresa; el cambio queda en el historial. [Migración: identificador histórico G-LIVE-017; referencias históricas: D-LIVE-022, D-LIVE-023]

### G-56: Sin ubicaciones

La primera versión no registra estantería, caja ni posición física exacta. [Migración: identificador histórico G-LIVE-018; referencias históricas: D-LIVE-028]

### G-58: Correcciones sin sobrescritura

Cada corrección conserva valor anterior, valor nuevo, actor, fecha y hora y motivo. Al cambiar un pedido confirmado a Compra personal, SANIA borra sus unidades del stock y las tareas de anuncio relacionadas tras pedir confirmación. [Migración: identificador histórico CORRECCION-AUDITABLE; referencias históricas: T03-Q04, T04-Q01, E-LIVE-005]

## 6. Estados

### clasificación aprendida del producto

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| sin regla aprendida | Víctor eligió Stock para venta para el producto (Víctor) → pasa a 'Stock para venta' · Víctor confirmó cambiar el pedido a Compra personal y sus unidades se borraron del stock activo (Víctor) → pasa a 'Compra personal' |
| Stock para venta | Víctor confirmó cambiar el pedido a Compra personal y sus unidades se borraron del stock activo (Víctor) → pasa a 'Compra personal' |
| Compra personal | Víctor cambió la clasificación del producto a Stock para venta (Víctor) → pasa a 'Stock para venta' |

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

Números del negocio:

| Qué | Cuánto |
|---|---|
| usuarios iniciales de esta actividad | 1, Víctor |

- Habla con **Confirmar la recepción de pedidos**: recibir exclusivamente paquetes con Todo correcto o mantener bloqueadas sus incidencias
- Habla con **Telegram**: clasificar productos, confirmar acciones y comunicar tareas de publicación; sin cadencias inventadas
- Habla con **Creación asistida de anuncios**: lanzar tareas inmediatamente después de crear unidades de Stock para venta
- Habla con **Excel de costes**: conservarlo como fuente histórica actual, no como autorización de fórmulas de pricing no confirmadas

## 8. Superficie de uso

### Inventario de Víctor

| Campo | Valor |
|---|---|
| Quién entra | Víctor |
| Por dónde llega | desde la confirmación Todo correcto y desde la consulta del inventario de SANIA |
| Cuándo lo usa | quiso revisar una entrada, unidad, clasificación o corrección |
| Qué ve nada más entrar | origen del pedido y paquete, producto, variante, referencia pública, estado, evidencia de coste e historial |
| Qué puede hacer | cambiar la clasificación aprendida · consultar unidades y tareas derivadas · solicitar una corrección explicando el motivo |
| Qué NO debe poder jamás | crear una unidad por el tracking · asignar una ubicación detallada · borrar el historial · aplicar automáticamente una fórmula de pricing, disputa, sustitución o reembolso no confirmada |

### Matriz de permisos

|  | cambiar la clasificación aprendida | consultar unidades y tareas derivadas | solicitar una corrección explicando el motivo |
|---|---|---|---|
| Víctor | ✓ | ✓ | ✓ |

## 9. Calidad y límites

- **Q-22**: Cero unidades creadas desde un tracking entregado sin Todo correcto.
- **Q-23**: Para N ejemplares físicamente confirmados y clasificados como Stock para venta existen exactamente N unidades y N referencias públicas únicas, incluso tras reprocesar el hecho; ninguna referencia se reutiliza.
- **Q-24**: Cero compras personales incorporadas al inventario de venta o a tareas de anuncio.
- **Q-25**: El 100 % de las correcciones admitidas por este flujo conserva antes, después, actor, fecha y hora y motivo.
- **Q-26**: Cero ubicaciones físicas detalladas registradas en la primera versión.
- **Q-27**: Al cambiar un pedido con Todo correcto a Compra personal, SANIA mostró todos los productos afectados y no borró ninguna unidad del stock activo sin una confirmación expresa; tras el sí, ninguna unidad afectada siguió en el stock, ninguna tarea de anuncio relacionada siguió activa y el cambio permaneció en el historial.

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

- (Ninguna por ahora.)

