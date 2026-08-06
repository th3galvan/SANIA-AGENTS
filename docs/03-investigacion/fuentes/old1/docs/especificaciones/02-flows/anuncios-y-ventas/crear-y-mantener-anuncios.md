# Spec: Crear y mantener anuncios

Proyecto `sania-crear-y-mantener-anuncios`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

## 1. Propósito

En la primera versión, SANIA prepara tareas de publicación independientes para Wallapop y Vinted y Víctor ejecuta manualmente toda creación o modificación en la plataforma. Cada anuncio representa una unidad física y su título termina con la referencia pública alfanumérica de tres caracteres de esa unidad; el enlace puede guardarse si se aporta, pero no es obligatorio para identificarla.

Cuando el stock quedó disponible para venta, Víctor necesitó recibir secuencialmente el par de plataformas de cada objetivo de publicación, disponer del texto listo para copiar y pegar, pedir las imágenes solo cuando las necesitó y confirmar de forma persistente la unidad y plataforma concretas sobre las que actuó.

Criterios de éxito:
- Cada acción de publicación o confirmación quedó resuelta a una unidad y una plataforma concretas; la granularidad con que nacen o se agrupan las tareas en una entrada múltiple siguió pendiente.
- El título preparado terminó con la referencia pública de tres caracteres de la unidad.
- Confirmar Anuncio creado persistió la plataforma y la unidad sin pedir un enlace obligatorio.
- SANIA no creó, editó, eliminó, reactivó ni publicó anuncios dentro de Wallapop o Vinted.

## 2. Actores y vocabulario

- **Víctor**: creó, modificó, retiró, reactivó y publicó manualmente los anuncios en Wallapop y Vinted
- **SANIA**: preparó contenido, generó tareas, entregó imágenes bajo demanda y registró las confirmaciones humanas

- "referencia de unidad": código alfanumérico de tres caracteres, único por unidad y visible como sufijo del título; el alfabeto, las colisiones, la reutilización y el agotamiento siguen pendientes
- "anuncio": publicación de una plataforma que representa una sola unidad física
- "tarea de publicación": mensaje o acción de Telegram que debe resolver una unidad y una plataforma antes de poder confirmarse, con contenido y botonera propios; puede formar parte de una generación agrupada cuya granularidad todavía no se decidió
- "Anuncio creado": confirmación humana que persiste que la unidad quedó publicada en la plataforma de la tarea concreta

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### Víctor publicó manualmente un objetivo mediante acciones independientes por plataforma [con la app]

Las tareas nacen después de que la unidad entre correctamente en stock para venta. La decisión de procesar una entrada grande de forma secuencial está confirmada, pero la granularidad final producto/unidad todavía debe aclararse.

- [automático: código] Para la acción concreta, SANIA resolvió la unidad que representaría el anuncio y preparó un título cuyo último elemento era su referencia alfanumérica de tres caracteres, una descripción veraz y los demás datos necesarios para copiar y pegar en la plataforma.
- [automático: código] SANIA presentó dos acciones independientes, Wallapop y Vinted, para el mismo objetivo producto/unidad y no pasó al siguiente objetivo hasta presentar ambas. Cada acción confirmable quedó ligada a una unidad y plataforma concretas; X-LIVE-011 mantiene pendiente si la tarea generadora se crea por producto o por unidad.
- [automático: código] Telegram entregó por defecto el texto listo para copiar y pegar y la botonera Enviar imágenes, Anuncio creado, Recordar más tarde y Cancelar sugerencia; no adjuntó las imágenes automáticamente.
- ⚑ Regla: ¿Cómo continuó Víctor desde la tarea concreta?
    - si Enviar imágenes:
        - [automático: código] SANIA envió las imágenes asociadas sin cerrar ni completar la tarea, que siguió disponible para una acción posterior.
        - …y vuelve al flujo
    - si Víctor publicó manualmente y pulsó Anuncio creado:
        - [persona] Víctor copió el contenido, creó el anuncio dentro de Wallapop o Vinted y volvió a la tarea. · Víctor
        - [automático: código] SANIA persistió que esa unidad estaba publicada en la plataforma deducida del mensaje y conservó la confirmación en el historial.
        - aquí termina este camino
    - si Recordar más tarde:
        - [automático: código] SANIA registró el aplazamiento sin inferir una publicación ni cambiar el stock; la cadencia y el momento del nuevo aviso siguen pendientes.
        - …y vuelve al flujo
    - si Cancelar sugerencia:
        - [automático: código] SANIA descartó la tarea de publicación sin inferir que existiera un anuncio ni cambiar el stock; queda pendiente decidir si produce algún efecto adicional sobre la intención de anunciar.
        - …y vuelve al flujo

### Víctor mejoró manualmente un anuncio [con la app]

- [persona] Víctor revisó dudas repetidas de compradores o la falta de interés y decidió añadir respuestas frecuentes o cambiar fotos y descripción. · Víctor
- [automático: código] SANIA pudo preparar una propuesta y conservar la versión y el motivo si Víctor los registró, pero no midió rendimiento con un umbral no definido ni aplicó el cambio en la plataforma.
- [persona] Víctor comprobó que el contenido siguiera siendo veraz, mantuvo la referencia al final del título y editó el anuncio manualmente. · Víctor

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- Víctor analizó conversaciones, convirtió preguntas repetidas en información de la descripción y cambió fotos o texto cuando un anuncio tenía poco interés; no se definieron métricas ni umbrales objetivos. [E-LIVE-006, D-LIVE-029, T09-Q02]
- Cuando se acordó un extra o lote, Víctor elevó la oferta y modificó manualmente el anuncio para que la descripción final reflejara exactamente lo vendido. [D-LIVE-002, G-LIVE-010, T01-Q06, T01-Q07]
- Una última unidad pudo estar anunciada en ambas plataformas; esta excepción no convirtió los dos anuncios en dos unidades distintas. [D-LIVE-008, D-LIVE-009, G-LIVE-008]

## 5. Reglas de negocio

### D-LIVE-020: Las tareas nacieron después de la recepción correcta

Al pulsar Todo correcto se registró el stock y se generaron inmediatamente las tareas de anuncio, pero solo para unidades clasificadas como stock para venta.

### G-LIVE-016: Las compras personales no generaron anuncios

D-LIVE-022 separó Stock para venta de Compra personal; únicamente la primera clasificación alimentó este flujo.

### D-LIVE-021: Las publicaciones de entradas grandes se presentaron secuencialmente

Se presentan los dos mensajes de plataforma de un producto/unidad antes de continuar con el siguiente. X-LIVE-011 mantiene abierta la granularidad exacta producto/unidad.

### D-LIVE-011: El texto llegó listo para copiar y pegar

Cada mensaje incluyó título con referencia, descripción y datos necesarios adaptados a su plataforma.

### G-LIVE-005: Un anuncio representó una unidad

D-LIVE-007 y T04-Q06 fijaron una vinculación anuncio-unidad uno a uno; varias unidades del mismo producto necesitaron anuncios distintos. Esta regla aplica al anuncio, mientras la granularidad de generación masiva de tareas sigue abierta en X-LIVE-011.

### G-LIVE-006: La referencia fue un sufijo público de tres caracteres

El título terminó con la referencia alfanumérica de la unidad. D-LIVE-006 y X-LIVE-001 sustituyeron la antigua referencia privada; X-LIVE-010 resolvió la repetición del título base mediante el sufijo, pendiente de validarlo en correos y movimientos reales.

### G-LIVE-009: Toda escritura en las plataformas fue humana

Según D-LIVE-010 y X-LIVE-003, SANIA solo preparó información y avisos; Víctor creó, editó, eliminó, reactivó y publicó manualmente.

### G-LIVE-010: El contenido enviado coincidió con la descripción final

Los extras acordados se incorporaron manualmente al anuncio antes de cerrar la oferta; una conversación por sí sola no sustituyó la descripción final.

### G-LIVE-013: Las tareas fueron independientes por plataforma

D-LIVE-013 y D-LIVE-016 dispusieron un mensaje para Wallapop y otro para Vinted, cada uno con Enviar imágenes, Anuncio creado, Recordar más tarde y Cancelar sugerencia y con estado separado.

### G-LIVE-014: Las imágenes se enviaron bajo demanda

D-LIVE-012 dispuso que Telegram no enviara imágenes por defecto y que Enviar imágenes las aportara cuando Víctor las solicitara.

### G-LIVE-015: Anuncio creado produjo una confirmación persistente

Según D-LIVE-014 y T07-Q01, la tarea concreta ya identificó unidad y plataforma para el único usuario inicial; pulsar Anuncio creado actualizó ese estado sin volver a identificar la unidad.

### D-LIVE-015: El enlace no fue requisito de identidad

La referencia del título permitió vincular la venta. Por X-LIVE-002, una URL solo puede conservarse como dato opcional para una finalidad futura de navegación o auditoría, nunca como requisito confirmado.

### D-LIVE-024: Variantes reutilizables fuera del MVP

En evolución se propuso generar 10 variantes, usar cada una hasta 3 veces y, al agotarse las 10, generar otro lote de 10. Ambos valores son configurables; antes deben normalizarse la ficha y la variante y revisarse las normas de plataforma.

### G-LIVE-020: La configurabilidad se decide caso a caso

D-LIVE-025 exige preguntar a Víctor antes de fijar como configurable una función cuando tenga sentido; X-LIVE-012 descarta convertir todo en configuración obligatoria.

### D-LIVE-027: Negociación y contraofertas aplazadas

La ayuda para negociar o proponer contraofertas queda fuera del MVP y no se incorpora hasta disponer de una vía permitida y reglas verificadas.

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| anuncio | plataforma, unidad física y referencia alfanumérica de tres caracteres, título exacto publicado con la referencia al final, descripción y versión de contenido, producto y variante, estado por plataforma: pendiente, publicado, pendiente de retirada o retirado cuando exista confirmación, fecha de creación de la tarea, fecha y evidencia de la confirmación humana disponible, URL solo si Víctor la aporta y sin convertirla en obligatoria, fecha inicial para medir tiempo hasta la venta solo cuando se defina su origen | ficha de producto, unidad, tarea por plataforma y confirmaciones de Víctor |
| tarea de publicación | unidad y plataforma ligadas, texto preparado, estado independiente, peticiones de imágenes, acciones de la botonera, historial y fechas | SANIA después de la entrada correcta en stock para venta |

- Habla con **Telegram**: entregar por plataforma el texto listo, enviar imágenes bajo demanda y recoger acciones de la tarea
- Habla con **Wallapop y Vinted**: servir como destino de la actuación manual de Víctor; SANIA no inició sesión ni escribió en ellas

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Crear, editar, retirar, reactivar o republicar automáticamente un anuncio.
- Responder o negociar con compradores en el MVP.
- Exigir una URL o inferir que los parámetros b, i y r son identificadores estables.
- Leer perfiles públicos, iniciar sesión o usar navegador automatizado como parte de este flujo.
- Promover al MVP la generación avanzada de variantes, el pricing completo o la negociación.
- Generar o variar imágenes, títulos o descripciones con la finalidad de eludir detección o controles de Wallapop o Vinted.
- Fijar una cadencia de Recordar más tarde o un efecto de Cancelar sugerencia más allá de descartar la tarea sin decisión adicional.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- T03-Q08 — ¿Qué alfabeto usa la referencia de tres caracteres y cómo se gestionan colisiones, reutilización y agotamiento?
- T04-Q02 / T04-Q05 / X-LIVE-004 — ¿Cómo identifica físicamente Víctor la unidad referenciada cuando varias unidades idénticas no llevan etiqueta ni rasgo diferenciador?
- T04-Q06 / D-LIVE-021 / X-LIVE-011 — ¿Las tareas de una entrada múltiple se generan por unidad o por producto, manteniendo en todo caso un anuncio por unidad?
- T07-Q04 / T07-Q05 / G-LIVE-015 — ¿Cuándo reaparece Recordar más tarde, qué efecto adicional tiene Cancelar sugerencia después de descartar la tarea y cómo se corrige Anuncio creado pulsado por error o una respuesta duplicada?
- G-LIVE-014 — ¿Qué formato y cuántas imágenes entrega Enviar imágenes?
- T09-Q05 / X-LIVE-002 — Si la URL se conserva opcionalmente, ¿para qué finalidad y qué ocurre cuando cambia o falla?
- T09-Q06 — ¿Wallapop y Vinted pueden llevar precios distintos y qué regla de redondeo se aplica?
- T01-Q10 — ¿Desde qué hecho empieza a medirse el tiempo hasta la venta?

