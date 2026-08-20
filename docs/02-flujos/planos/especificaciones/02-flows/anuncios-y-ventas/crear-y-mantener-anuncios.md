# Spec: Crear y mantener anuncios

Proyecto `sania-crear-y-mantener-anuncios`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

En la primera versión, SANIA prepara una tarea de publicación por producto y plataforma y Víctor ejecuta manualmente toda creación o modificación en Wallapop y Vinted. Si hay al menos dos unidades disponibles, los anuncios reciben referencias distintas; si solo hay una, ambos comparten esa referencia. El orden de elección no importa, no se crean publicaciones duplicadas y la venta descuenta la referencia ya asignada al anuncio.

Cuando el stock quedó disponible para venta, Víctor necesitó recibir una tarea por producto y plataforma, disponer del texto listo para copiar y pegar, pedir las imágenes solo cuando las necesitó y confirmar de forma persistente la plataforma y las unidades relacionadas sobre las que actuó.

Criterios de éxito:
- Cada producto con una o varias unidades idénticas generó exactamente una tarea para Wallapop y otra para Vinted, con estado independiente.
- Al preparar un anuncio, SANIA le asignó una referencia disponible sin aplicar un orden obligatorio y el título terminó con esa referencia.
- Víctor creó personalmente el anuncio y Anuncio creado solo persistió la publicación después de que SANIA recibiera su enlace.
- SANIA no creó, editó, eliminó, reactivó ni publicó anuncios dentro de Wallapop o Vinted.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

- "referencia de unidad": identificador lógico único que empieza con tres letras y también aparece como sufijo público del anuncio. Usa el alfabeto explícito Z, Y, ..., A, z, y, ..., a; comienza en ZZZ, no se reutiliza nunca y, al agotarse una longitud, añade una letra y reinicia desde el valor máximo, por ejemplo ZZZZ. Antes del etiquetado físico individual, la referencia puede satisfacerse con cualquiera de las unidades idénticas disponibles; después del etiquetado identifica una unidad física concreta
- "anuncio": publicación manual de un producto en Wallapop o Vinted. Solo puede existir un anuncio activo del mismo producto en cada plataforma. Al crear el anuncio, SANIA le asigna una referencia disponible del producto sin que importe el orden; la venta descuenta esa referencia ya asignada. El título ayuda a conciliar, pero nunca es la única fuente de verdad
- "tarea de publicación": propuesta de publicar un producto en una plataforma. Cancelar sugerencia descarta la tarea y desactiva nuevas sugerencias automáticas para ese producto y plataforma. Víctor puede reactivarlas manualmente con Volver a sugerir, que crea inmediatamente una tarea si hay stock y no existe un anuncio activo allí; si publica por su cuenta mientras están desactivadas, debe vincular manualmente el anuncio con el producto y la referencia
- "Anuncio creado": acción que solicita el enlace del anuncio creado personalmente por Víctor; SANIA no lo marca como publicado hasta recibir ese enlace
- "El enlace es correcto": acción disponible cuando SANIA cuestiona un enlace; acepta el enlace actual como correcto y cierra esa comprobación sin volver a preguntarlo

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### Víctor publicó manualmente un objetivo mediante acciones independientes por plataforma [con la app · origen: usuario]

Las tareas nacen después de que las unidades entren correctamente en stock para venta. Se crea una tarea y como máximo un anuncio activo por producto y plataforma, aunque existan varias unidades idénticas: con Wallapop y Vinted son exactamente dos tareas y dos anuncios activos.

- [automático: código] Para el producto concreto, SANIA eligió cualquiera de las referencias disponibles y la asignó al anuncio al prepararlo; el título terminó con esa referencia.
- [automático: código] SANIA presentó exactamente dos tareas independientes para el producto: una para Wallapop y otra para Vinted. Asignó referencias distintas si había al menos dos disponibles y la misma referencia a ambas únicamente si había una.
- [automático: código] Telegram entregó por defecto el texto listo para copiar y pegar y la botonera Enviar imágenes, Anuncio creado, Recordar más tarde y Cancelar sugerencia; no adjuntó las imágenes automáticamente.
- ⚑ Regla: ¿Cómo continuó Víctor desde la tarea concreta?
    - si Enviar imágenes:
        - [automático: código] SANIA envió las imágenes asociadas al producto sin cerrar ni completar la tarea, que siguió disponible para una acción posterior.
        - …y vuelve al flujo
    - si Víctor publicó manualmente y pulsó Anuncio creado:
        - [persona] Víctor copió el contenido, creó el anuncio dentro de Wallapop o Vinted y volvió a la tarea. · Víctor
        - [automático: código] SANIA solicitó el enlace del anuncio y mantuvo la tarea pendiente; pulsar el botón por sí solo no cambió el estado de publicación.
        - [persona] Víctor envió a SANIA el enlace del anuncio que acababa de crear. · Víctor
        - [automático: código] Si SANIA no cuestionó el enlace, lo guardó, persistió que el producto y la referencia asignada estaban publicados en la plataforma de la tarea y conservó la confirmación en el historial. Si lo cuestionó, aplicó el recorrido de resolución del enlace cuestionado.
        - aquí termina este camino
    - si Recordar más tarde:
        - [automático: código] SANIA registró el aplazamiento sin inferir una publicación ni cambiar el stock, mantuvo la tarea pendiente y volvió a presentarla a las 18:00 del día siguiente, según su hora local.
        - …y vuelve al flujo
    - si Cancelar sugerencia:
        - [automático: código] SANIA descartó la tarea, no cambió el stock ni marcó el producto como publicado y desactivó nuevas sugerencias automáticas para ese producto en esa plataforma.
        - …y vuelve al flujo

### Víctor mejoró manualmente un anuncio [con la app · origen: usuario]

- [persona] Víctor revisó dudas repetidas de compradores o la falta de interés y decidió añadir respuestas frecuentes o cambiar fotos y descripción. · Víctor
- [automático: código] SANIA pudo preparar una propuesta y conservar la versión y el motivo si Víctor los registró, pero no midió rendimiento con un umbral no definido ni aplicó el cambio en la plataforma.
- [persona] Víctor comprobó que el contenido siguiera siendo veraz, mantuvo la referencia al final del título y editó el anuncio manualmente. · Víctor

### SANIA creó una nueva tarea para la plataforma después de una venta [con la app · origen: usuario]

- [tercero externo] Una venta cerró el anuncio de Wallapop o Vinted y reservó la referencia que tenía asignada.
- ⚑ Regla: ¿Quedaba alguna unidad disponible y las sugerencias seguían activas para ese producto y plataforma?
    - si no:
        - [automático: código] SANIA no creó otra tarea porque no quedaba stock o Víctor había cancelado las sugerencias para ese producto y plataforma.
        - aquí termina este camino
    - camino normal: sí, SANIA creó automáticamente una nueva tarea para la misma plataforma
- [automático: código] SANIA asignó a la nueva tarea otra referencia disponible. Si tras la venta quedaban al menos dos unidades, usó una referencia distinta de la anunciada en la otra plataforma; si quedaba una sola, compartió esa referencia.
- [persona] Víctor publicó manualmente el nuevo anuncio desde la tarea. · Víctor

### SANIA borró automáticamente las tareas de anuncio después de retirar el producto del stock [con la app · origen: usuario]

- [automático: código] Víctor confirmó cambiar un pedido a Compra personal después de revisar los productos que saldrían del stock.
- [automático: código] SANIA retiró las unidades del stock y borró automáticamente todas las tareas de anuncio relacionadas en la misma operación.
- [automático: código] Ninguna tarea de Wallapop o Vinted relacionada con esos productos siguió apareciendo entre las tareas activas.

### Víctor vinculó un anuncio creado por su cuenta [con la app · origen: usuario]

- [persona] Víctor publicó por su cuenta un anuncio de un producto para el que SANIA había dejado de sugerir en esa plataforma. · Víctor
- [persona] Desde SANIA eligió el producto, la plataforma y la referencia correspondiente y vinculó manualmente el anuncio. · Víctor
- [automático: código] SANIA guardó la relación y el historial sin afirmar que la publicación procediera de una sugerencia automática.

### Víctor corrigió el enlace asociado a un anuncio [con la app · origen: usuario]

- ⚑ Regla: ¿Desde dónde corrigió Víctor el enlace?
    - si Desde SANIA:
        - [persona] Víctor abrió la ficha del anuncio, eligió Corregir enlace e introdujo el enlace correcto. · Víctor
        - …y vuelve al flujo
    - si Desde Telegram:
        - [persona] Víctor usó la acción Corregir enlace de un mensaje ligado al anuncio y envió el enlace correcto. · Víctor
        - …y vuelve al flujo
- [automático: código] Si SANIA no cuestionó el enlace nuevo, actualizó el enlace del mismo anuncio, eliminó completamente el anterior sin conservarlo en el historial y no modificó su producto, plataforma, referencia ni el stock. Si lo cuestionó, aplicó el recorrido de resolución del enlace cuestionado.

### Víctor resolvió un enlace cuestionado por SANIA [con la app · origen: usuario]

- [automático: código] SANIA mantuvo el contexto de la misma tarea o anuncio y mostró que cuestionaba el enlace recibido.
- ⚑ Regla: ¿Cómo resolvió Víctor el enlace cuestionado?
    - si Envió por mensaje el enlace correcto:
        - [persona] Víctor envió el enlace correcto en un mensaje. · Víctor
        - [automático: código] SANIA aceptó ese enlace para la misma tarea o anuncio y no volvió a pedir confirmación para él.
        - aquí termina este camino
    - si Pulsó El enlace es correcto:
        - [automático: código] SANIA aceptó el enlace actual para la misma tarea o anuncio y no volvió a pedir confirmación para él.
        - aquí termina este camino

### Víctor confirmó un enlace repetido [con la app · origen: usuario]

- [automático: código] SANIA detectó que había recibido exactamente el mismo enlace para la misma tarea y avisó a Víctor.
- ⚑ Regla: ¿Víctor confirmó que estaba seguro de usar ese enlace repetido?
    - si no:
        - [automático: código] SANIA conservó el estado que ya tenía y no trató el enlace repetido como una confirmación.
        - aquí termina este camino
    - camino normal: sí, SANIA lo aceptó como si Víctor hubiera pulsado El enlace es correcto y no volvió a pedir confirmación para él

### Víctor reactivó las sugerencias de un producto en una plataforma [con la app · origen: usuario]

- [persona] Víctor abrió el producto y la plataforma cuyas sugerencias había cancelado y pulsó Volver a sugerir. · Víctor
- [automático: código] SANIA reactivó las sugerencias automáticas solo para ese producto y plataforma, sin modificar el stock ni publicar un anuncio.
- ⚑ Regla: ¿Había stock disponible y no existía un anuncio activo de ese producto en la plataforma reactivada?
    - si no:
        - [automático: código] SANIA mantuvo activas las sugerencias, pero no creó una tarea porque no había stock disponible o ya existía un anuncio activo en esa plataforma.
        - aquí termina este camino
    - camino normal: sí, SANIA creó inmediatamente una tarea de publicación y le asignó una referencia disponible conforme a las reglas del producto

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

### REC-6: Mostrar la referencia pública al final del título (pendiente · 1ª entrega)

Permitir reconocer la unidad asignada al anuncio sin depender de una referencia privada ni de una URL.

- **R-16**: Cada título termina con la referencia pública que SANIA asignó al anuncio al crearlo, elegida entre las disponibles sin un orden obligatorio. · regla G-39 · origen: usuario · código actual: no verificado

- **C-16**: Dado un producto con una o varias referencias públicas disponibles / Cuando SANIA preparó el título del anuncio / Entonces SANIA asignó una referencia disponible al anuncio y el último elemento del título fue exactamente esa referencia, no una referencia privada ni una URL · cubre R-16

### REC-4: Borrar automáticamente tareas de anuncio de productos retirados del stock (pendiente)

Evitar que queden tareas de publicación activas para productos que ya no cuentan como stock para venta.

- **R-13**: Al confirmar el cambio de un pedido a Compra personal, SANIA borra automáticamente todas las tareas de anuncio relacionadas con sus productos retirados del stock. · regla G-93 · origen: usuario · código actual: no verificado
- **R-14**: La retirada del stock y el borrado de todas las tareas relacionadas ocurren en la misma operación confirmada, sin una segunda acción manual. · regla G-93 · origen: usuario · código actual: no verificado

- **C-13**: Dado un pedido cambiado a Compra personal cuyas unidades ya salieron del stock / Cuando SANIA terminó la corrección / Entonces ninguna tarea de anuncio relacionada siguió activa · cubre R-13
- **C-14**: Dado un pedido con tareas activas para Wallapop y Vinted / Cuando Víctor confirmó cambiarlo a Compra personal / Entonces SANIA retiró las unidades del stock y borró las dos tareas relacionadas en la misma operación · cubre R-14

### REC-7: Crear una tarea por producto y plataforma (pendiente)

Evitar tareas repetidas cuando se reciben varias unidades idénticas del mismo producto.

- **R-17**: Una o varias unidades idénticas del mismo producto generan una sola tarea y un solo anuncio activo para Wallapop, y una sola tarea y un solo anuncio activo para Vinted; cada anuncio recibe una referencia disponible al crearse. · regla G-36 · origen: usuario · código actual: no verificado
- **R-18**: Cada tarea conserva la plataforma, el producto y la referencia asignada. Con al menos dos unidades, Wallapop y Vinted usan referencias distintas; con una sola unidad, comparten esa referencia. · regla G-42 · origen: usuario · código actual: no verificado
- **R-19**: Tras vender la referencia anunciada en una plataforma, si queda stock y las sugerencias siguen activas, SANIA crea una nueva tarea para esa plataforma con otra referencia disponible. · regla G-94 · origen: usuario · código actual: no verificado

- **C-17**: Dado una entrada con varias unidades idénticas del mismo producto / Cuando SANIA generó las tareas de publicación / Entonces existieron exactamente dos tareas y como máximo dos anuncios activos: uno para Wallapop y otro para Vinted, sin duplicados dentro de cada plataforma · cubre R-17
- **C-18**: Dado las dos tareas creadas para un producto con varias unidades idénticas / Cuando se consultó cada tarea / Entonces cada una mostró su plataforma y su referencia; fueron distintas si había al menos dos unidades y coincidieron únicamente si había una · cubre R-18
- **C-19**: Dado una venta que cerró el anuncio de una plataforma mientras quedaba stock y las sugerencias seguían activas / Cuando SANIA registró la venta y reservó la referencia asignada / Entonces creó automáticamente una nueva tarea para esa misma plataforma con otra referencia disponible, sin publicar el anuncio por sí misma · cubre R-19

### REC-8: Controlar sugerencias y vincular una publicación manual (pendiente)

Respetar la decisión de no recibir más propuestas automáticas, permitir reactivarlas manualmente y conservar la posibilidad de registrar un anuncio creado por iniciativa de Víctor.

- **R-20**: Cancelar sugerencia desactiva nuevas tareas automáticas para el mismo producto y plataforma, sin cambiar el stock ni marcar el producto como publicado. · regla G-95 · origen: usuario · código actual: no verificado
- **R-21**: Si Víctor publica por su cuenta después de cancelar las sugerencias, puede vincular manualmente el anuncio con el producto y la referencia correspondiente. · regla G-95 · origen: usuario · código actual: no verificado
- **R-22**: Víctor puede pulsar Volver a sugerir para reactivar manualmente las sugerencias automáticas del mismo producto y plataforma que había cancelado; si hay stock disponible y no existe un anuncio activo allí, SANIA crea inmediatamente una tarea de publicación. · regla G-95 · origen: usuario · código actual: no verificado

- **C-20**: Dado una tarea de publicación activa para un producto y plataforma / Cuando Víctor pulsó Cancelar sugerencia / Entonces la tarea se descartó y SANIA no volvió a generar sugerencias automáticas para ese producto y plataforma ni modificó el stock · cubre R-20
- **C-21**: Dado un anuncio publicado por Víctor fuera de una sugerencia automática / Cuando Víctor lo vinculó manualmente en SANIA / Entonces el anuncio quedó relacionado con el producto, la plataforma y la referencia indicados por Víctor · cubre R-21
- **C-22**: Dado un producto y una plataforma con las sugerencias automáticas desactivadas / Cuando Víctor pulsó Volver a sugerir / Entonces SANIA reactivó las sugerencias automáticas únicamente para ese producto y plataforma y, al haber stock y no existir un anuncio activo allí, creó inmediatamente una tarea de publicación sin modificar el stock ni publicar el anuncio · cubre R-22

### REC-9: Recordar una tarea de publicación al día siguiente (pendiente)

Permitir aplazar una tarea sin perderla ni interpretar que el anuncio fue publicado.

- **R-23**: Recordar más tarde mantiene pendiente la tarea de publicación y hace que SANIA vuelva a presentarla a las 18:00 del día siguiente, según la hora local de SANIA, sin modificar el stock ni el estado del anuncio. · regla G-96 · origen: usuario · código actual: no verificado

- **C-23**: Dado una tarea de publicación pendiente / Cuando Víctor pulsó Recordar más tarde / Entonces SANIA conservó la tarea sin cambiar el stock ni marcar el anuncio como publicado y volvió a presentarla a las 18:00 del día siguiente, según su hora local · cubre R-23

### REC-10: Confirmar un anuncio creado personalmente por Víctor (pendiente)

Registrar la publicación manual con su enlace sin que una pulsación aislada cambie el estado.

- **R-24**: Víctor crea personalmente el anuncio; al pulsar Anuncio creado, SANIA solicita su enlace y solo marca como publicado el producto, plataforma y referencia de la tarea cuando recibe ese enlace. · regla G-44 · origen: usuario · código actual: no verificado
- **R-25**: Víctor puede corregir el enlace de un anuncio tanto desde un lugar específico en SANIA como mediante una acción de Telegram ligada al mismo anuncio; SANIA elimina completamente el enlace anterior sin conservarlo en el historial. · regla G-98 · origen: usuario · código actual: no verificado
- **R-26**: Si SANIA cuestiona un enlace, Víctor puede enviar por mensaje el enlace correcto o pulsar El enlace es correcto; cualquiera de las dos acciones lo acepta definitivamente y SANIA no vuelve a pedir confirmación para ese enlace. · regla G-99 · origen: usuario · código actual: no verificado
- **R-27**: Si SANIA recibe dos veces exactamente el mismo enlace para la misma tarea, avisa y pide confirmación; si Víctor confirma que sí, lo trata como El enlace es correcto y no vuelve a preguntarlo. · regla G-100 · origen: usuario · código actual: no verificado

- **C-24**: Dado una tarea de publicación cuyo anuncio creó personalmente Víctor / Cuando Víctor pulsó Anuncio creado y envió el enlace solicitado / Entonces SANIA guardó el enlace y marcó como publicados el producto, la plataforma y la referencia ya ligados a la tarea; si no recibió el enlace, mantuvo la tarea pendiente · cubre R-24
- **C-25**: Dado un anuncio publicado con un enlace equivocado / Cuando Víctor aportó el enlace correcto desde la ficha del anuncio en SANIA o desde la acción correspondiente en Telegram / Entonces SANIA sustituyó el enlace del mismo anuncio, eliminó completamente el anterior sin conservarlo en el historial y no cambió el producto, la plataforma, la referencia ni el stock · cubre R-25
- **C-26**: Dado un enlace cuestionado por SANIA / Cuando Víctor envió por mensaje el enlace correcto o pulsó El enlace es correcto / Entonces SANIA aceptó el enlace aplicable a la misma tarea o anuncio y no volvió a solicitar confirmación para ese enlace · cubre R-26
- **C-27**: Dado el mismo enlace recibido por segunda vez para la misma tarea / Cuando Víctor confirmó que estaba seguro / Entonces SANIA aceptó el enlace como si Víctor hubiera pulsado El enlace es correcto y no volvió a solicitar confirmación para él · cubre R-27

### REC-11: Enviar las imágenes asociadas al producto (pendiente)

Permitir que Víctor reciba bajo demanda las imágenes ya asociadas al producto sin modificar la tarea ni el estado de publicación.

- **R-28**: Cada producto guarda sus imágenes, descripción, precio, precio máximo y precio mínimo aceptable en negociación; al pulsar Enviar imágenes, SANIA envía las imágenes asociadas a ese producto sin cambiar el estado de la tarea. · regla G-101 · origen: usuario · código actual: no verificado

- **C-28**: Dado una tarea de publicación de un producto con imágenes asociadas / Cuando Víctor pulsó Enviar imágenes / Entonces SANIA envió las imágenes asociadas al producto y mantuvo sin cambios el estado de la tarea, el stock y la publicación · cubre R-28

### REC-12: Aplicar el precio común del producto en las plataformas (pendiente)

Mantener una referencia de precio coherente entre Wallapop y Vinted sin inventar una diferencia automática por plataforma.

- **R-29**: Por defecto, las publicaciones de Wallapop y Vinted usan el mismo precio de publicación y el mismo precio mínimo aceptable en negociación asociados al producto. · regla G-102 · origen: usuario · código actual: no verificado

- **C-29**: Dado un producto con tareas de publicación en Wallapop y Vinted / Cuando SANIA preparó los datos de ambas publicaciones / Entonces ambas recibieron el mismo precio de publicación y el mismo precio mínimo aceptable en negociación asociados al producto, sin aplicar una subida automática para Vinted · cubre R-29

### REC-13: Aplicar una excepción de precio superior en Vinted (pendiente)

Permitir un precio de publicación mayor en Vinted sin alterar el mínimo de negociación común del producto.

- **R-30**: Un producto puede tener en Vinted un precio de publicación superior al precio común de Wallapop y Vinted, sin cambiar su precio mínimo aceptable en negociación. · regla G-103 · origen: usuario · código actual: no verificado

- **C-30**: Dado un producto con una excepción de precio de publicación en Vinted / Cuando SANIA preparó sus datos de publicación / Entonces Vinted recibió el precio superior registrado y Wallapop conservó el precio común, mientras que ambas plataformas mantuvieron el mismo mínimo aceptable en negociación · cubre R-30

### REC-14: Fijar manualmente el precio superior de Vinted (pendiente)

Permitir a Víctor decidir libremente el importe de publicación de Vinted cuando no quiera usar el precio común.

- **R-31**: Víctor fija arbitrariamente por producto el precio de publicación superior de Vinted; SANIA no calcula ni sugiere fórmula, porcentaje o recargo automático. · regla G-104 · origen: usuario · código actual: no verificado

- **C-31**: Dado un producto para el que Víctor quiso publicar más caro en Vinted / Cuando Víctor fijó el importe de Vinted / Entonces SANIA usó exactamente ese importe para Vinted sin calcular ni proponer una diferencia automática frente a Wallapop y conservó el mínimo común de negociación · cubre R-31

### REC-15: Revisar el precio específico de Vinted tras cambiar el común (pendiente)

Evitar que un precio de Vinted fijado manualmente siga considerándose revisado después de cambiar el precio común del producto.

- **R-32**: Cuando cambia el precio común de un producto que tiene precio específico de Vinted, SANIA marca ese precio de Vinted como pendiente de revisión manual sin recalcularlo ni modificarlo automáticamente. · regla G-105 · origen: usuario · código actual: no verificado

- **C-32**: Dado un producto con precio específico de Vinted ya fijado / Cuando Víctor cambió el precio común del producto / Entonces SANIA mantuvo el importe específico sin recalcularlo y lo marcó como pendiente de revisión manual · cubre R-32

### REC-16: Avisar y mostrar la revisión pendiente de Vinted (pendiente)

Informar a Víctor sin crear una tarea cuando el cambio de precio común deja pendiente la revisión del importe específico de Vinted.

- **R-33**: Cuando el precio específico de Vinted queda pendiente de revisión, SANIA no crea una tarea ni recordatorio, envía un aviso inmediato por Telegram y lo muestra en el dashboard. · regla G-106 · origen: usuario · código actual: no verificado

- **C-33**: Dado un producto cuyo precio común cambió y tenía precio específico de Vinted / Cuando SANIA marcó ese precio como pendiente de revisión / Entonces no creó tarea ni recordatorio, envió un aviso inmediato por Telegram y mostró el pendiente en el dashboard · cubre R-33

### REC-17: Tratar como intercambiables las unidades idénticas sin etiqueta (pendiente)

Permitir que Víctor entregue cualquier unidad idéntica disponible sin exigir una correspondencia física imposible con la referencia lógica del anuncio.

- **R-34**: Cuando varias unidades idénticas no tienen etiqueta ni rasgo diferenciador, SANIA las trata como intercambiables: no exige identificar físicamente la referencia asignada al anuncio y la venta descuenta esa referencia lógica. · regla G-107 · origen: usuario · código actual: no verificado

- **C-34**: Dado varias unidades idénticas del mismo producto sin etiqueta ni rasgo diferenciador / Cuando Víctor entrega una unidad tras vender el anuncio / Entonces pudo entregar cualquier unidad idéntica disponible y SANIA descontó la referencia lógica asignada al anuncio, sin pedir una identificación física · cubre R-34

### REC-18: Medir desde la confirmación de publicación (pendiente)

Calcular el tiempo hasta la venta desde el instante en que SANIA confirma que el anuncio se publicó con su enlace.

- **R-35**: SANIA inicia la medición del tiempo hasta la venta cuando recibe y guarda el enlace con el que confirma la publicación del anuncio; no la inicia al crear la tarea ni al pulsar Anuncio creado sin enlace. · regla G-108 · origen: usuario · código actual: no verificado
- **R-36**: Si Víctor corrige el enlace de un anuncio ya publicado, SANIA mantiene la fecha original de confirmación de publicación y no reinicia la medición del tiempo hasta la venta. · regla G-108 · origen: usuario · código actual: no verificado

- **C-35**: Dado una tarea de publicación creada para un producto y plataforma / Cuando Víctor pulsó Anuncio creado y después SANIA recibió y guardó el enlace del anuncio / Entonces SANIA inició la medición en el instante de guardar ese enlace, no al crear la tarea ni al pulsar el botón · cubre R-35
- **C-36**: Dado un anuncio cuya publicación ya fue confirmada y cuya medición de tiempo ya comenzó / Cuando Víctor corrigió su enlace desde SANIA o Telegram / Entonces SANIA sustituyó el enlace pero conservó la fecha original de confirmación de publicación y continuó la misma medición · cubre R-36

### Episodios reales que sustentan los requisitos

- Víctor analizó conversaciones, convirtió preguntas repetidas en información de la descripción y cambió fotos o texto cuando un anuncio tenía poco interés; no se definieron métricas ni umbrales objetivos. [Migración: identificador histórico E-LIVE-006; referencias históricas: E-LIVE-006, D-LIVE-029, T09-Q02]
- Cuando se acordó un extra o lote, Víctor elevó la oferta y modificó manualmente el anuncio para que la descripción final reflejara exactamente lo vendido. [Migración: identificador histórico E-LIVE-002; referencias históricas: D-LIVE-002, T01-Q06, T01-Q07] [G-41]
- Una última unidad pudo estar anunciada en ambas plataformas; esta excepción no convirtió los dos anuncios en dos unidades distintas. [Migración: identificador histórico E-LIVE-003; referencias históricas: D-LIVE-008, D-LIVE-009, G-LIVE-008]

## 5. Reglas de negocio

### G-34: Las tareas nacieron después de la recepción correcta

Al pulsar Todo correcto se registró el stock y se generaron inmediatamente las tareas de anuncio, pero solo para unidades clasificadas como stock para venta. [Migración: identificador histórico D-LIVE-020]

### G-35: Las compras personales no generaron anuncios

D-LIVE-022 separó Stock para venta de Compra personal; únicamente la primera clasificación alimentó este flujo. [Migración: identificador histórico G-LIVE-016]

### G-36: Una tarea por producto y plataforma

Aunque entren varias unidades idénticas del mismo producto, SANIA crea una sola tarea para Wallapop y otra para Vinted. Si hay al menos dos referencias disponibles, asigna una distinta a cada plataforma; si solo hay una, ambas tareas comparten esa referencia. El orden de elección no importa. [Migración: identificador histórico D-LIVE-021; decisión cerrada de X-LIVE-011 y T04-Q06]

### G-37: El texto llegó listo para copiar y pegar

Cada mensaje incluyó título con referencia, descripción y datos necesarios adaptados a su plataforma. [Migración: identificador histórico D-LIVE-011]

### G-38: Un único anuncio activo por producto y plataforma

Cada unidad conserva su referencia y su trazabilidad. Si hay al menos dos unidades, Wallapop y Vinted apuntan a referencias distintas; únicamente cuando queda una sola ambos anuncios apuntan a la misma. Una única tarea por producto y plataforma corresponde a un solo anuncio activo. [Migración: sustituye el antecedente histórico G-LIVE-005; decisión cerrada de X-LIVE-011 y T04-Q06]

### G-39: La referencia fue un sufijo público no reutilizable

El título terminó con la referencia pública que SANIA asignó al anuncio al crearlo. Puede elegir cualquier referencia disponible porque el orden no importa. La secuencia empieza en ZZZ, usa Z, Y, ..., A, z, y, ..., a, nunca reutiliza un valor y añade una letra al agotar una longitud. [Migración: identificador histórico G-LIVE-006; referencias históricas: D-LIVE-006, T03-Q08, X-LIVE-001]

### G-40: Toda escritura en las plataformas fue humana

Según D-LIVE-010 y X-LIVE-003, SANIA solo preparó información y avisos; Víctor creó, editó, eliminó, reactivó y publicó manualmente. [Migración: identificador histórico G-LIVE-009]

### G-41: El contenido enviado coincidió con la descripción final

Los extras acordados se incorporaron manualmente al anuncio antes de cerrar la oferta; una conversación por sí sola no sustituyó la descripción final. [Migración: identificador histórico G-LIVE-010]

### G-42: Las tareas fueron independientes por plataforma

D-LIVE-013 y D-LIVE-016 dispusieron un mensaje para Wallapop y otro para Vinted, cada uno con Enviar imágenes, Anuncio creado, Recordar más tarde y Cancelar sugerencia y con estado separado. [Migración: identificador histórico G-LIVE-013]

### G-43: Las imágenes se enviaron bajo demanda

Telegram no envía imágenes por defecto. Enviar imágenes aporta las que estén asociadas al producto cuando Víctor las solicita. El formato y la cantidad se definirán al decidir su generación y deberán ser compatibles con Wallapop y Vinted. [Migración: identificador histórico G-LIVE-014; referencias históricas: D-LIVE-012]

### G-44: Anuncio creado solicita el enlace antes de confirmar

Víctor crea personalmente el anuncio. La tarea ya identifica producto, plataforma y referencia; al pulsar Anuncio creado, SANIA solicita el enlace y solo después de recibirlo marca el anuncio como publicado y guarda la confirmación. Pulsar el botón sin aportar el enlace no cambia el estado. [Migración: identificador histórico G-LIVE-015; referencias históricas: D-LIVE-014, T07-Q01]

### G-45: El enlace confirma la publicación pero no sustituye su identidad

SANIA conserva el enlace aportado por Víctor como evidencia obligatoria de la publicación. El producto, la plataforma y la referencia proceden de la tarea; la URL no los sustituye ni convierte los parámetros b, i y r en identificadores estables. [Migración: identificador histórico D-LIVE-015; referencia histórica: X-LIVE-002]

### G-46: Variantes reutilizables fuera del MVP

En evolución se propuso generar 10 variantes, usar cada una hasta 3 veces y, al agotarse las 10, generar otro lote de 10. Ambos valores son configurables; antes deben normalizarse la ficha y la variante y revisarse las normas de plataforma. [Migración: identificador histórico D-LIVE-024; estado histórico: evolución, no MVP; referencias históricas: X-LIVE-009]

### G-47: La configurabilidad se decide caso a caso

D-LIVE-025 exige preguntar a Víctor antes de fijar como configurable una función cuando tenga sentido; X-LIVE-012 descarta convertir todo en configuración obligatoria. [Migración: identificador histórico G-LIVE-020; estado histórico: principio de diseño; referencias históricas: D-LIVE-025, X-LIVE-012]

### G-48: Negociación y contraofertas aplazadas

La ayuda para negociar o proponer contraofertas queda fuera del MVP y no se incorpora hasta disponer de una vía permitida y reglas verificadas. [Migración: identificador histórico D-LIVE-027; estado histórico: evolución aplazada]

### G-93: Las tareas de anuncio se borran automáticamente

Cuando un pedido confirmado pasa a Compra personal y sus unidades salen del stock, SANIA borra automáticamente todas las tareas de anuncio relacionadas en la misma operación.

### G-94: Una venta con stock repone la tarea de la plataforma

Cuando se vende la referencia anunciada en una plataforma y todavía quedan unidades disponibles, SANIA crea una nueva tarea para esa plataforma salvo que Víctor haya cancelado las sugerencias del producto allí. La publicación sigue siendo manual.

### G-95: Víctor controla las sugerencias por producto y plataforma

Cancelar sugerencia descarta la tarea y hace que SANIA deje de generar sugerencias automáticas para ese producto en esa plataforma. Volver a sugerir permite reactivarlas manualmente y crea de inmediato una tarea para ese producto y plataforma cuando hay stock disponible y no existe allí un anuncio activo. Ninguna de las dos acciones cambia el stock ni publica un anuncio. Si Víctor publica por su cuenta mientras están desactivadas, vincula manualmente el anuncio con el producto y su referencia.

### G-96: Recordar más tarde aplaza hasta el día siguiente

Cuando Víctor pulsa Recordar más tarde en una tarea de publicación, SANIA conserva la tarea sin cambiar el stock ni el estado de publicación y la vuelve a presentar a las 18:00 del día siguiente, según la hora local de SANIA.

### G-98: El enlace del anuncio puede corregirse desde SANIA o Telegram

Víctor puede sustituir el enlace asociado a un anuncio desde su ficha en SANIA o mediante una acción de Telegram ligada al mismo anuncio. Ambos canales actualizan el mismo dato y no cambian el producto, la plataforma, la referencia ni el stock. Al sustituirlo, SANIA elimina completamente el enlace anterior y no lo conserva en el historial.

### G-99: Víctor resuelve definitivamente un enlace cuestionado

Cuando SANIA cuestiona un enlace, Víctor puede enviar por mensaje el enlace correcto o pulsar El enlace es correcto. Cualquiera de las dos acciones acepta el enlace para la tarea o el anuncio ligado y SANIA no vuelve a solicitar confirmación para ese enlace.

### G-100: Un enlace repetido requiere una confirmación explícita

Si SANIA recibe por segunda vez exactamente el mismo enlace para la misma tarea, avisa de que ya lo recibió y pregunta a Víctor si está seguro. Si confirma que sí, equivale a pulsar El enlace es correcto: acepta el enlace y no vuelve a preguntarlo. Si responde que no, conserva el estado que ya tenía sin tratar el duplicado como confirmación.

### G-101: Cada producto reúne su contenido y sus límites de precio

Cada producto conserva sus imágenes, descripción, precio, precio máximo y precio mínimo aceptable en negociación. SANIA usa las imágenes asociadas al producto al ejecutar Enviar imágenes; el formato y la cantidad se decidirán al definir su generación, con compatibilidad para Wallapop y Vinted.

### G-102: El precio y el mínimo de negociación son comunes por defecto

Por defecto, Wallapop y Vinted usan el mismo precio de publicación y el mismo precio mínimo aceptable en negociación del producto. Vinted puede tener un precio de publicación mayor por producto; el mínimo de negociación permanece común. La forma de fijar esa subida sigue pendiente.

### G-103: Vinted puede tener un precio de publicación superior

SANIA permite registrar para un producto un precio de publicación superior en Vinted respecto al precio común. Víctor decide ese importe arbitrariamente por producto; SANIA no aplica fórmula, porcentaje ni recargo automático. Esta excepción no cambia el precio mínimo aceptable en negociación, que sigue siendo el mismo para Wallapop y Vinted.

### G-104: Víctor fija libremente el precio superior de Vinted

Cuando un producto use un precio de publicación mayor en Vinted, Víctor introduce el importe que estime para ese producto. SANIA conserva el valor elegido sin calcular ni sugerir una diferencia automática frente a Wallapop.

### G-105: Un cambio del precio común exige revisar el precio de Vinted

Si cambia el precio común del producto, el precio específico de Vinted pasa a requerir revisión manual de Víctor. SANIA no lo recalcula ni lo ajusta automáticamente, no crea una tarea ni recordatorio, y envía un aviso inmediato por Telegram mientras muestra el pendiente en el dashboard.

### G-106: La revisión pendiente de Vinted se comunica sin crear tarea

Marcar el precio específico de Vinted como pendiente de revisión no crea una tarea ni recordatorio. SANIA envía un aviso inmediato por Telegram y muestra el pendiente en el dashboard para que Víctor lo revise posteriormente.

### G-107: Las unidades idénticas sin etiqueta son intercambiables

Cuando varias unidades del mismo producto son idénticas y no tienen etiqueta ni rasgo diferenciador, no es necesario identificar físicamente cuál corresponde a cada referencia. La referencia conserva la trazabilidad lógica del anuncio y la venta descuenta la referencia que ya tiene asignada, aunque Víctor pueda entregar cualquier unidad idéntica disponible.

### G-108: La medición de venta comienza al confirmar la publicación

SANIA empieza a medir el tiempo hasta la venta cuando confirma la publicación del anuncio al recibir y guardar el enlace que Víctor le aporta. Crear la tarea, preparar el texto o pulsar Anuncio creado sin enlace no inicia la medición. Corregir después el enlace del mismo anuncio conserva ese instante inicial y no reinicia la medición.

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| producto para publicación | imágenes asociadas al producto, descripción del producto, precio de publicación común por defecto para Wallapop y Vinted, con posibilidad de precio superior específico para Vinted fijado arbitrariamente por Víctor, sin fórmula ni recargo automático, y estado pendiente de revisión cuando cambie el precio común, notificado inmediatamente por Telegram y mostrado en el dashboard sin crear una tarea ni recordatorio, precio máximo, precio mínimo aceptable en negociación común para Wallapop y Vinted, también cuando Vinted usa un precio de publicación superior, formato y cantidad de imágenes pendientes de decidir al definir su generación, con compatibilidad requerida para Wallapop y Vinted | datos del producto confirmados o preparados para su publicación |
| anuncio | plataforma, unidad física y referencia pública de longitud inicial tres, no reutilizable, título exacto publicado con la referencia asignada al final, descripción y versión de contenido, producto y variante, estado por plataforma: pendiente, publicado, pendiente de retirada o retirado cuando exista confirmación, fecha de creación de la tarea, fecha y evidencia de la confirmación humana disponible, URL vigente aportada obligatoriamente por Víctor para confirmar la publicación; su estado de aceptación cuando Víctor pulsó El enlace es correcto para no volver a cuestionarla; al corregirla solo se conserva la nueva y se elimina completamente la anterior, sin usar ninguna URL como identidad canónica del producto, la plataforma o la referencia, fecha y hora original en que SANIA recibió y guardó el enlace que confirmó la publicación, que inicia la medición del tiempo hasta la venta y se conserva si posteriormente se corrige el enlace | ficha de producto, unidad, tarea por plataforma y confirmaciones de Víctor |
| tarea de publicación | producto y plataforma ligados, referencia disponible asignada al crear el anuncio, texto preparado, estado independiente, peticiones de imágenes, acciones de la botonera, historial y fechas, sugerencias automáticas activas o desactivadas para el producto y plataforma | SANIA después de la entrada correcta en stock para venta |

- Habla con **Telegram**: entregar por plataforma el texto listo, enviar imágenes bajo demanda, recoger acciones de la tarea, solicitar el enlace antes de confirmar una publicación, permitir corregirlo después y resolver un enlace cuestionado
- Habla con **Wallapop y Vinted**: servir como destino de la actuación manual de Víctor; SANIA no inició sesión ni escribió en ellas

## 8. Superficie de uso

### Tareas de anuncio

| Campo | Valor |
|---|---|
| Quién entra | Víctor |
| Por dónde llega | desde la lista de tareas o la ficha de un anuncio en SANIA |
| Cuándo lo usa | quiso revisar una tarea de publicación o un anuncio ya confirmado |
| Qué ve nada más entrar | el producto, la plataforma, la referencia asignada y, si ya fue confirmado, su enlace |
| Qué puede hacer | consultar una tarea de anuncio activa · confirmar un anuncio creado personalmente y aportar su enlace · corregir el enlace de un anuncio · confirmar que un enlace cuestionado es correcto · confirmar el uso de un enlace repetido · cancelar sugerencias para el producto y plataforma · volver a sugerir para el producto y plataforma · vincular manualmente un anuncio con producto, plataforma y referencia |
| Qué NO debe poder jamás | mostrar como activa una tarea cuyo pedido confirmado pasó a Compra personal |

### Matriz de permisos

|  | consultar una tarea de anuncio activa | confirmar un anuncio creado personalmente y aportar su enlace | corregir el enlace de un anuncio | confirmar que un enlace cuestionado es correcto | confirmar el uso de un enlace repetido | cancelar sugerencias para el producto y plataforma | volver a sugerir para el producto y plataforma | vincular manualmente un anuncio con producto, plataforma y referencia |
|---|---|---|---|---|---|---|---|---|
| Víctor | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

## 9. Calidad y límites

- **Q-29**: El 100 % de las tareas de anuncio relacionadas se borró automáticamente al confirmar el cambio de un pedido a Compra personal.
- **Q-31**: El 100 % de los títulos preparados terminó con la referencia pública asignada al crear el anuncio y ninguna referencia se decidió por primera vez al registrar la venta.
- **Q-32**: Cada producto generó exactamente dos tareas y como máximo un anuncio activo por plataforma: referencias distintas con dos o más unidades disponibles y una referencia compartida únicamente con una sola unidad.
- **Q-33**: El 100 % de las ventas que dejaron stock y mantenían activas las sugerencias generó una nueva tarea para la misma plataforma, con otra referencia y sin publicar automáticamente.
- **Q-34**: Tras Cancelar sugerencia, se generaron cero nuevas tareas automáticas para ese producto y plataforma; cualquier anuncio posterior solo quedó registrado mediante una vinculación manual de Víctor.
- **Q-35**: Volver a sugerir reactivó las sugerencias únicamente para el producto y plataforma elegidos y, cuando había stock y no existía allí un anuncio activo, creó inmediatamente una sola tarea sin modificar el stock ni publicar automáticamente.
- **Q-36**: El 100 % de las tareas de publicación aplazadas con Recordar más tarde siguió pendiente y reapareció a las 18:00 del día siguiente, según la hora local de SANIA, sin alterar el stock ni el estado de publicación.
- **Q-37**: El 100 % de las publicaciones confirmadas conservó el enlace aportado por Víctor; ninguna pulsación de Anuncio creado sin enlace marcó el anuncio como publicado.
- **Q-38**: El enlace de un anuncio pudo corregirse tanto desde SANIA como desde Telegram y ambos canales actualizaron el mismo anuncio sin cambiar producto, plataforma, referencia ni stock; el enlace anterior quedó completamente eliminado y no apareció en el historial.
- **Q-39**: El 100 % de los enlaces cuestionados quedó aceptado al recibir por mensaje un enlace correcto o pulsar El enlace es correcto, y SANIA no volvió a solicitar confirmación para ese enlace.
- **Q-40**: El 100 % de los enlaces recibidos por segunda vez para la misma tarea generó un aviso y una confirmación; al aceptar, SANIA los trató como El enlace es correcto y no volvió a preguntar por ellos.
- **Q-41**: El 100 % de las acciones Enviar imágenes entregó únicamente las imágenes asociadas al producto de la tarea y no alteró su estado, el stock ni la publicación.
- **Q-42**: El 100 % de las publicaciones preparadas sin excepción explícita usó en Wallapop y Vinted el mismo precio de publicación y el mismo mínimo de negociación asociados al producto.
- **Q-43**: El 100 % de las excepciones de precio en Vinted aplicó el precio de publicación superior del producto sin cambiar el mínimo aceptable en negociación compartido con Wallapop.
- **Q-44**: El 100 % de los precios superiores de Vinted coincidió exactamente con el importe fijado por Víctor, sin fórmula, porcentaje, recargo ni sugerencia automática de SANIA.
- **Q-45**: El 100 % de los cambios del precio común en productos con precio específico de Vinted dejó ese importe sin modificar y marcado como pendiente de revisión manual.
- **Q-46**: El 100 % de los precios específicos de Vinted marcados como pendientes de revisión no creó tarea ni recordatorio, envió un aviso inmediato por Telegram y apareció en el dashboard.
- **Q-47**: El 100 % de las ventas de productos con unidades idénticas sin etiqueta permitió entregar cualquier unidad disponible sin exigir correspondencia física con la referencia lógica del anuncio.
- **Q-48**: El 100 % de las mediciones de tiempo hasta la venta comenzó al guardar el enlace que confirmó la publicación y no antes.
- **Q-49**: El 100 % de las correcciones de enlace conservó la fecha de confirmación original y no reinició la medición del tiempo hasta la venta.

## 10. Fuera de alcance

- Crear, editar, retirar, reactivar o republicar automáticamente un anuncio.
- Crear varias publicaciones activas del mismo producto dentro de una plataforma.
- Responder o negociar con compradores en el MVP.
- Usar la URL como identidad canónica del producto, la plataforma o la referencia, o inferir que los parámetros b, i y r son identificadores estables.
- Leer perfiles públicos, iniciar sesión o usar navegador automatizado como parte de este flujo.
- Promover al MVP la generación avanzada de variantes, el pricing completo o la negociación.
- Generar o variar imágenes, títulos o descripciones con la finalidad de eludir detección o controles de Wallapop o Vinted.
- Reactivar automáticamente, sin acción de Víctor, sugerencias canceladas.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- G-LIVE-014 — Al definir la generación de imágenes, ¿qué formato y cuántas imágenes serán compatibles con Wallapop y Vinted?

