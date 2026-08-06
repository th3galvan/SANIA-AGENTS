# Informe monolítico de entrevista SANIA — LIVE

> Documento de transferencia para que otro agente actualice los planos funcionales de SANIA. Consolida el contexto de partida, la entrevista, los correos aportados, la captura del monedero, decisiones, dudas, contradicciones e ideas. No contiene diseño técnico ni modifica por sí mismo los planos.

## 0. Identificación

- **Proyecto:** SANIA
- **Fuente base:** `PAQUETE-COMPLETO-PARA-CHATGPT-LIVE.md`
- **Commit de referencia:** `024c8e7`
- **Fecha del paquete base:** 29/07/2026
- **Fecha de este informe:** 31/07/2026
- **Actor entrevistado:** Víctor
- **Canales tratados:** Gmail de solo lectura, Telegram, Wallapop, Vinted, AliExpress, transportistas, Excel y monedero de Wallapop.
- **Evidencias adjuntas examinadas:** 6 correos `.eml` y 1 captura del historial del monedero de Wallapop.
- **Temas tratados:** venta, reserva, preparación, envío, cierre, recepción AliExpress, stock, publicaciones, referencias, Telegram, variantes de anuncio y pricing futuro.
- **Temas no cerrados:** identificadores AliExpress/Wallapop, cierre automático de Wallapop, costes completos, concurrencia, tickets, tiempos, volumen, permisos sensibles, devoluciones y extravíos reales.

### Criterio de fidelidad

- Se separan **decisiones**, **casos reales**, **reglas**, **hipótesis**, **contradicciones** y **pendientes**.
- Las respuestas “nunca me ha pasado” no se convierten en reglas operativas.
- Las propuestas hechas por el entrevistador solo se consideran decisión cuando Víctor las confirmó de forma suficientemente clara.
- Se omiten únicamente muletillas, interrupciones y repeticiones literales de la misma pregunta; el apéndice conserva todas las preguntas y respuestas sustantivas.
- Se minimizan datos personales de compradores y direcciones: los planos necesitan campos y finalidades, no identidades reales.

## 1. Resumen ejecutivo

La entrevista cambia de forma importante el modelo anterior de identificación de unidades y anuncios. Víctor decidió que cada unidad física tendrá una **referencia alfanumérica de tres caracteres**, visible al final del título, y que cada anuncio representará una unidad. Esto sustituye la regla previa de referencia privada y hace innecesario, en principio, pegar el enlace del anuncio para vincularlo.

El flujo central queda así: el correo inicial de venta reserva la unidad; si es la última y está publicada en ambas plataformas, SANIA avisa para retirar manualmente el otro anuncio. SANIA no escribe en Wallapop ni Vinted. Para AliExpress, el estado “entregado” no suma stock: Víctor comprueba físicamente contenido y cantidades y confirma por Telegram. Solo entonces se crean las unidades y tareas de publicación.

Vinted sí proporciona un correo final inequívoco. Wallapop no: el correo de entrega mantiene la venta abierta y el cierre económico solo se ve en el monedero. Este es el **bloqueo principal**: no se ha decidido cómo recibirá SANIA el cierre de Wallapop si su acceso se limita a correos.

También se definió un flujo de publicación asistida por Telegram, con texto listo para copiar, imágenes bajo demanda y botones de confirmación. Las ideas de variantes generadas, rotación de títulos/descripciones, precios por márgenes y negociación deben mantenerse como evolución y no incorporarse silenciosamente a la primera versión.

## 2. Cobertura

| ID | Estado | Evidencia breve | Falta |
|---|---|---|---|
| `T01-Q01` | **resuelto** | Caso real completo desde correo de venta, consulta de conversación/anuncio, preparación física, QR, admisión, seguimiento y cierre. | Faltan ejemplos alternativos por plataforma y transportista. |
| `T01-Q02` | **parcial** | El título del anuncio será el campo visible de enlace y terminará en una referencia alfanumérica de 3 caracteres de la unidad. | Validar con correos reales que incluyan el nuevo sufijo y definir tratamiento de títulos alterados. |
| `T01-Q03` | **sin tratar** | Se observaron parámetros b, i y r en un enlace de Wallapop, pero no se explicó su significado ni estabilidad. | Ejemplos comparados y prueba de estabilidad. |
| `T01-Q04` | **aplazado** | Víctor no recuerda cancelaciones reales. | Correo real de cancelación, identificadores y efectos. |
| `T01-Q05` | **parcial** | Los correos muestran nombre abreviado del comprador; se usa para informar y reconocer la operación. | Definir qué datos se conservan, finalidad, visibilidad y retención. |
| `T01-Q06` | **parcial** | Se describieron extras y lotes acordados, pero no una venta real con varias unidades del mismo producto. | Caso real multiunidad/multiproducto y reparto de importes. |
| `T01-Q07` | **resuelto** | El extra se acuerda, se eleva la oferta y se modifica el anuncio para que la descripción refleje exactamente lo vendido. | Confirmar si existe algún límite o excepción. |
| `T01-Q08` | **resuelto** | Si queda una unidad puede estar anunciada en ambas plataformas; el primer correo reserva y obliga a retirar manualmente el otro anuncio. | Definir plazo, prueba de retirada y colisión de correos simultáneos. |
| `T01-Q09` | **parcial** | Ante cancelación, SANIA solo avisaría para reactivar/republicar manualmente. | No hay caso real ni correo de cancelación. |
| `T01-Q10` | **sin tratar** | No se definió el instante inicial de medición del tiempo hasta la venta. | Elegir publicación, primera visibilidad u otro hecho. |
| `T01-Q11` | **parcial** | Hay ejemplos de unidad vinculada y última unidad; no hay casos reales de FIFO, falta de stock, duplicado o cancelación. | Aportar ejemplos reales o mantenerlos como simulaciones no confirmadas. |
| `T02-Q01` | **parcial** | Se aportaron plantillas reales: Wallapop venta confirmada y paquete entregado; Vinted venta inicial y cierre final. | Faltan AliExpress, Correos/InPost de admisión y otros estados, además de cancelación. |
| `T02-Q02` | **sin tratar** | No se identificó un campo estable de línea de AliExpress. | Correo/página real con identificadores de pedido, línea, producto y paquete. |
| `T02-Q03` | **parcial** | Se explicó que un pedido puede dividirse, consolidarse y llegar a cuentagotas. | Plantillas, identificadores y transiciones reales de compra, envío y división. |
| `T02-Q04` | **parcial** | La admisión se prueba normalmente con correo del transportista; el tránsito se mira por tracking y hay correo de entrega. | Ejemplos de intento fallido, incidencias y diferencias por transportista. |
| `T02-Q05` | **aplazado** | Víctor no recuerda correos duplicados, tardíos o desordenados. | Ejemplos reales; mantener protección de idempotencia del contexto. |
| `T02-Q06` | **parcial** | Los correos aportados tienen asuntos y códigos reconocibles, como TX-COMPLETE en Vinted. | Definir patrones deterministas y campos obligatorios por plantilla. |
| `T02-Q07` | **sin tratar** | No se definió cuándo una IA puede proponer datos extraídos de correos ni qué revisión exige. | Casos permitidos, confianza y aprobación humana. |
| `T02-Q08` | **parcial** | Un paquete no correcto o una lectura dudosa debe quedar como incidencia/disputa sin sumar stock. | Regla general para correos desconocidos o contradictorios. |
| `T02-Q09` | **parcial** | Los pedidos parciales se mantienen abiertos hasta la comprobación física de todos los paquetes. | Definir qué datos y estados se muestran mientras están incompletos. |
| `T03-Q01` | **parcial** | Caso real de paquetes divididos; defectuoso, incompleto o variante errónea no han ocurrido y se abriría disputa. | Primer caso real de fallo y resultado económico. |
| `T03-Q02` | **parcial** | Actualmente se cuentan e identifican productos manualmente contra el pedido. | Campos exactos para distinguir líneas similares. |
| `T03-Q03` | **parcial** | SANIA debe recordar la comprobación hasta resolverla y ofrecer No volver a recordar. | Cadencia inicial, escalado y significado exacto de silenciar. |
| `T03-Q04` | **parcial** | Hoy el error se corrige manualmente en Excel; se pidió tratar por texto un sistema robusto. | Flujo auditable de deshacer/corregir una confirmación. |
| `T03-Q05` | **sin tratar** | No se definieron precisión interna ni formato visible del coste unitario. | Decimales, redondeo y moneda. |
| `T03-Q06` | **parcial** | El precio se calculará desde coste, margen de publicación y margen mínimo. | Definir si difiere por plataforma y fórmula exacta. |
| `T03-Q07` | **sin tratar** | No se fijaron día ni hora del recordatorio semanal de disputa. | Cadencia concreta. |
| `T03-Q08` | **resuelto** | Referencia alfanumérica de 3 caracteres, única por unidad, visible al final del título. | Definir alfabeto, colisiones, reutilización y agotamiento. |
| `T03-Q09` | **parcial** | Se habló hipotéticamente de devolución y disputa; hay un reembolso visible en el monedero. | Casos reales de reembolso, sustitución, pérdida y parcial. |
| `T04-Q01` | **parcial** | Cuando detecta un fallo de stock, Víctor ajusta el Excel manualmente. | Último caso concreto, antes/después, motivo y efecto económico. |
| `T04-Q02` | **parcial** | Las unidades idénticas se buscan visualmente en una caja y no tienen diferencias notables. | Resolver cómo una referencia única identifica físicamente una unidad sin etiqueta. |
| `T04-Q03` | **parcial** | El contexto separa stock y movimiento económico; la entrevista solo describió ajustes manuales. | Caso real que afecte stock, ajuste y dinero de forma distinta. |
| `T04-Q04` | **parcial** | Primera venta por correo reserva la unidad compartida y exige retirar el otro anuncio. | Regla de concurrencia para dos correos casi simultáneos. |
| `T04-Q05` | **resuelto** | Cada unidad recibe su referencia de 3 caracteres; normalmente cada plataforma apunta a una unidad distinta. | Método de generación y gestión de colisiones. |
| `T04-Q06` | **resuelto** | Un anuncio representa una única unidad física. | Aclarar mensajes por producto frente a mensajes por unidad en lotes grandes. |
| `T05-Q01` | **parcial** | Víctor encuentra la unidad visualmente en la caja; el título y descripción indican qué se vendió. | Datos mínimos de una eventual tarea de preparación; Víctor rechazó un recordatorio genérico. |
| `T05-Q02` | **parcial** | Los extras se incorporan modificando el anuncio; no hay caso real completo multiunidad. | Ejemplo con varios productos/unidades y embalaje. |
| `T05-Q03` | **sin tratar** | No se describieron errores de preparación que abren incidencia. | Ejemplos y bloqueo operativo. |
| `T05-Q04` | **parcial** | El transportista envía correo de admisión; se muestra un QR en el punto de entrega. | Plantillas reales por transportista y relación con tracking. |
| `T05-Q05` | **parcial** | La recepción física se confirma manualmente con Todo correcto; otros usos de confirmación manual no se concretaron. | Admisión, entrega y cierre manuales permitidos. |
| `T05-Q06` | **sin tratar** | No se trató una confirmación manual seguida de correo contradictorio. | Regla de prioridad, ticket y auditoría. |
| `T05-Q07` | **sin tratar** | No se definió espera antes de recordar admisión. | Umbral temporal. |
| `T05-Q08` | **sin tratar** | No se fijaron acciones a las 48 horas. | Acciones concretas y destinatarios. |
| `T05-Q09` | **parcial** | Si el tracking no cambia, se espera y después se contacta al transportista y se informa al comprador. | Umbral exacto para ticket de extravío. |
| `T06-Q01` | **resuelto** | Vinted cierra con el correo La transacción se ha completado, código TX-COMPLETE, número de transacción, fecha e importes. | Confirmar posibles variantes de plantilla/idioma. |
| `T06-Q02` | **parcial** | Wallapop no envía correo final; el cierre se observa en el historial del monedero por transacción asociada al anuncio. | Definir cómo llega ese hecho a SANIA si solo lee correos. |
| `T06-Q03` | **parcial** | Si falta confirmación se consulta tracking, se espera, se contacta al transportista y se avisa al cliente. | Número de días y diferencias por estado/plataforma. |
| `T06-Q04` | **parcial** | Una incidencia de transporte o paquete parado impide considerar normal el cierre. | Lista completa de incidencias y criterios de desbloqueo. |
| `T06-Q05` | **sin tratar** | No se definieron comisiones, portes, embalajes, impuestos y demás gastos. | Fuentes, obligatoriedad y cálculo. |
| `T06-Q06` | **sin tratar** | No se trató el reparto de gastos entre unidades o ventas. | Reglas y ejemplos. |
| `T06-Q07` | **parcial** | Correos prueban precio de artículo/venta; el monedero prueba movimientos; Excel es referencia de costes. | Prueba de comisiones, embalajes, impuestos y otros importes. |
| `T06-Q08` | **sin tratar** | No se definieron cambios económicos con segunda confirmación. | Qué importes son sensibles y quién confirma. |
| `T06-Q09` | **parcial** | Hoy se corrige manualmente en Excel; el contexto exige auditoría antes/después/actor/fecha/motivo. | Flujo de corrección y ejemplos reales. |
| `T06-Q10` | **sin tratar** | No se aportaron cálculo normal y cálculo raro completos. | Dos ventas reales con todos los costes. |
| `T07-Q01` | **resuelto** | Víctor es el único usuario inicial; los botones pertenecen al mensaje/tarea concreta. | Mecanismo técnico de identificación fuera del alcance funcional. |
| `T07-Q02` | **resuelto** | SANIA registra hechos de correo y prepara tareas; Víctor confirma hechos físicos y ejecuta acciones en plataformas. | Definir cada excepción y nivel de autonomía. |
| `T07-Q03` | **sin tratar** | No se enumeraron propuestas que requieren segunda confirmación. | Lista de decisiones sensibles. |
| `T07-Q04` | **parcial** | Hay Recordar más tarde, Cancelar sugerencia y No volver a recordar; no se resolvieron respuestas duplicadas o contradictorias. | Semántica exacta de cada botón y resolución de conflictos. |
| `T07-Q05` | **parcial** | Los avisos pueden repetirse hasta resolución. | Cadencias concretas por tipo. |
| `T07-Q06` | **sin tratar** | No se fijó hora del recordatorio diario de retirada. | Hora y zona horaria. |
| `T08-Q01` | **parcial** | Aparecen estados de sugerencia, incidencia y disputa, pero no un catálogo completo de tickets/prioridades. | Estados, prioridades y transiciones. |
| `T08-Q02` | **parcial** | Víctor es responsable inicial; no se trataron cambios de responsabilidad. | Regla futura si hay más actores. |
| `T08-Q03` | **parcial** | Se definieron recordatorios, recordar más tarde y silenciar. | Plazos exactos y escalados. |
| `T08-Q04` | **parcial** | No OK bloquea la entrada en stock; Todo correcto permite stock y anuncios; incidencias bloquean cierre. | Matriz completa de bloqueos y continuaciones. |
| `T08-Q05` | **sin tratar** | No se definió evidencia de retirada de todos los anuncios. | Confirmación manual, captura o lectura pública permitida. |
| `T09-Q01` | **parcial** | Se propuso Playwright, pero después se afirmó que SANIA solo debe leer correos y no actuar en plataformas. | Decidir si existe alguna lectura pública sin sesión o queda descartada. |
| `T09-Q02` | **parcial** | Se identificaron título, imagen, importe, fecha y tipo de movimiento en el monedero. | Información pública mínima del anuncio y finalidad. |
| `T09-Q03` | **sin tratar** | No se trató bloqueo o CAPTCHA. | Procedimiento manual de reactivación. |
| `T09-Q04` | **sin tratar** | No se fijó revisión semanal. | Día, hora y alcance. |
| `T09-Q05` | **parcial** | El enlace dejó de ser necesario para identificar la unidad; no se definió qué hacer si un enlace se usa y falla. | Confirmar eliminación del enlace de los planos o flujo de error. |
| `T09-Q06` | **parcial** | Existe precio calculado, pero no se decidió si Wallapop y Vinted conservan precios distintos. | Regla por plataforma y redondeo. |
| `T10-Q01` | **sin tratar** | No se midió volumen actual. | Correos, pedidos, unidades, ventas y decisiones por día/semana. |
| `T10-Q02` | **sin tratar** | No se trató concurrencia general de dos hechos sobre la misma operación. | Ejemplos y orden de aplicación. |
| `T10-Q03` | **sin tratar** | No se definieron datos delicados ni personas sin acceso. | Clasificación y permisos. |
| `T10-Q04` | **sin tratar** | No se trató caída durante medio día ni contingencia. | Procedimiento manual y recuperación. |
| `T10-Q05` | **sin tratar** | No se definieron esperas tolerables. | Tiempos por acción y canal. |
| `T10-Q06` | **sin tratar** | No se documentaron dispositivos, idioma o dificultades de uso. | Entorno real de uso. |
| `T11-Q01` | **aplazado** | No existe primera devolución real; se habló hipotéticamente de inspección y reventa. | Correos, estados, dinero y resultado reales. |
| `T11-Q02` | **aplazado** | No existe procedimiento real de extravío documentado por plataforma. | Primer caso y comunicaciones. |
| `T11-Q03` | **parcial** | Las acciones en plataformas son manuales y SANIA notifica; no hay decisión completa para devoluciones/extravíos. | Confirmar que permanecen fuera del MVP hasta evidencia. |

## 3. Decisiones confirmadas

### D-LIVE-001 — Reserva desde el primer correo

- **Tema:** Venta/stock
- **Decisión:** Reserva desde el primer correo.
- **Evidencia:** Al reconocer el primer correo de venta, la unidad queda reservada.
- **Alcance:** Aplica a Wallapop y Vinted; evita doble asignación.
- **Planos afectados:** Ventas, stock, eventos externos.

### D-LIVE-002 — Contenido enviado según el anuncio

- **Tema:** Venta/preparación
- **Decisión:** Contenido enviado según el anuncio.
- **Evidencia:** Si no hay cambios, se envía lo descrito; un extra acordado debe incorporarse al anuncio antes de cerrar la oferta.
- **Alcance:** La conversación por sí sola no sustituye la descripción final del anuncio.
- **Planos afectados:** Anuncios, venta, preparación.

### D-LIVE-003 — Obtención manual de transportista y QR

- **Tema:** Envío
- **Decisión:** Obtención manual de transportista y QR.
- **Evidencia:** Víctor entra desde el correo o conversación, ve instrucciones, transportista y QR, y lo presenta en el punto de entrega.
- **Alcance:** Proceso humano actual.
- **Planos afectados:** Envíos, tareas físicas.

### D-LIVE-004 — Cierre de Vinted por correo final

- **Tema:** Cierre
- **Decisión:** Cierre de Vinted por correo final.
- **Evidencia:** El correo La transacción se ha completado es la evidencia final de Vinted.
- **Alcance:** Debe extraer número de transacción, fecha, título e importes.
- **Planos afectados:** Ventas, movimientos económicos.

### D-LIVE-005 — Wallapop no cierra por correo

- **Tema:** Cierre
- **Decisión:** Wallapop no cierra por correo.
- **Evidencia:** El correo de paquete entregado deja pendiente el OK del comprador; el cierre económico aparece en el monedero.
- **Alcance:** SANIA no puede inferir cierre solo con ese correo.
- **Planos afectados:** Ventas, cierre, conciliación.

### D-LIVE-006 — Referencia visible de tres caracteres

- **Tema:** Unidades/anuncios
- **Decisión:** Referencia visible de tres caracteres.
- **Evidencia:** Cada unidad física tiene una referencia alfanumérica de 3 caracteres visible al final del título.
- **Alcance:** Sustituye la regla previa de referencia privada.
- **Planos afectados:** Unidades, anuncios, vinculación de correos.

### D-LIVE-007 — Un anuncio por unidad

- **Tema:** Anuncios/stock
- **Decisión:** Un anuncio por unidad.
- **Evidencia:** Cada anuncio representa una sola unidad física.
- **Alcance:** La referencia del título identifica esa unidad.
- **Planos afectados:** Anuncios, unidades.

### D-LIVE-008 — Asignación entre plataformas

- **Tema:** Anuncios/stock
- **Decisión:** Asignación entre plataformas.
- **Evidencia:** Con varias unidades, Wallapop y Vinted apuntan normalmente a referencias distintas; con una sola, ambos pueden apuntar a la misma.
- **Alcance:** Excepción controlada para última unidad.
- **Planos afectados:** Anuncios, disponibilidad.

### D-LIVE-009 — Retirada manual tras venta de última unidad

- **Tema:** Venta/anuncios
- **Decisión:** Retirada manual tras venta de última unidad.
- **Evidencia:** Al llegar el primer correo de venta, SANIA avisa de retirar el anuncio de la otra plataforma; Víctor lo hace manualmente.
- **Alcance:** No hay escritura automática sobre Wallapop/Vinted.
- **Planos afectados:** Telegram, anuncios, stock.

### D-LIVE-010 — Acciones de plataforma siempre humanas

- **Tema:** Permisos
- **Decisión:** Acciones de plataforma siempre humanas.
- **Evidencia:** Víctor crea, modifica, elimina, reactiva y publica anuncios manualmente.
- **Alcance:** SANIA solo prepara información y notifica.
- **Planos afectados:** Permisos, autonomía.

### D-LIVE-011 — Texto listo para copiar y pegar

- **Tema:** Creación de anuncios
- **Decisión:** Texto listo para copiar y pegar.
- **Evidencia:** El mensaje de publicación incluye título con referencia, descripción y datos necesarios.
- **Alcance:** Contenido adaptado a cada plataforma.
- **Planos afectados:** Telegram, anuncios.

### D-LIVE-012 — Imágenes bajo demanda

- **Tema:** Creación de anuncios
- **Decisión:** Imágenes bajo demanda.
- **Evidencia:** Las imágenes no se envían por defecto; un botón Enviar imágenes las entrega cuando Víctor las solicita.
- **Alcance:** Evita saturar Telegram.
- **Planos afectados:** Telegram, archivos de producto.

### D-LIVE-013 — Botonera de publicación

- **Tema:** Creación de anuncios
- **Decisión:** Botonera de publicación.
- **Evidencia:** Cada sugerencia incluye Enviar imágenes, Anuncio creado, Recordar más tarde y Cancelar sugerencia.
- **Alcance:** La semántica exacta de recordatorios sigue parcial.
- **Planos afectados:** Telegram, tareas.

### D-LIVE-014 — Confirmación de publicación persistente

- **Tema:** Anuncios
- **Decisión:** Confirmación de publicación persistente.
- **Evidencia:** Al pulsar Anuncio creado, se guarda que la unidad está publicada en Wallapop o Vinted.
- **Alcance:** La plataforma se deduce del mensaje concreto.
- **Planos afectados:** Anuncios, unidad, historial.

### D-LIVE-015 — No se necesita enlace para identificar la unidad

- **Tema:** Anuncios
- **Decisión:** No se necesita enlace para identificar la unidad.
- **Evidencia:** El título preparado contiene la referencia; el correo de venta permite vincular la unidad.
- **Alcance:** Reemplaza el flujo previo basado en pegar el enlace, salvo que se decida conservarlo por otra finalidad.
- **Planos afectados:** Anuncios, Telegram, vinculación.

### D-LIVE-016 — Dos tareas separadas por plataforma

- **Tema:** Creación de anuncios
- **Decisión:** Dos tareas separadas por plataforma.
- **Evidencia:** SANIA envía un mensaje para Wallapop y otro para Vinted, cada uno con su botonera.
- **Alcance:** Permite completar una plataforma sin bloquear la otra.
- **Planos afectados:** Telegram, anuncios.

### D-LIVE-017 — Entrada en stock tras comprobación física

- **Tema:** Recepción
- **Decisión:** Entrada en stock tras comprobación física.
- **Evidencia:** El tracking entregado no suma stock; Víctor debe comprobar producto y cantidades y pulsar Todo correcto.
- **Alcance:** Protege contra paquetes incompletos o erróneos.
- **Planos afectados:** Paquetes, stock, auditoría.

### D-LIVE-018 — Recepción no correcta abre incidencia

- **Tema:** Recepción
- **Decisión:** Recepción no correcta abre incidencia.
- **Evidencia:** El mensaje de recepción ofrece Todo correcto o Abrir disputa; No OK no entra en stock.
- **Alcance:** El flujo interno de disputa sigue pendiente.
- **Planos afectados:** Paquetes, tickets, stock.

### D-LIVE-019 — Recordatorios de recepción

- **Tema:** Telegram
- **Decisión:** Recordatorios de recepción.
- **Evidencia:** Si Víctor no responde, SANIA recuerda la comprobación y ofrece No volver a recordar.
- **Alcance:** Cadencia por definir.
- **Planos afectados:** Telegram, tareas.

### D-LIVE-020 — Publicación inmediatamente después del OK

- **Tema:** Recepción/anuncios
- **Decisión:** Publicación inmediatamente después del OK.
- **Evidencia:** Al confirmar Todo correcto, se registra stock y se generan las tareas de publicación sin paso intermedio de ubicación.
- **Alcance:** El almacén actual no usa ubicaciones.
- **Planos afectados:** Stock, anuncios.

### D-LIVE-021 — Procesamiento secuencial de publicaciones

- **Tema:** Telegram
- **Decisión:** Procesamiento secuencial de publicaciones.
- **Evidencia:** Para entradas grandes: dos mensajes de un producto/unidad, luego los dos del siguiente.
- **Alcance:** Debe aclararse si la granularidad final es producto o unidad.
- **Planos afectados:** Telegram, anuncios.

### D-LIVE-022 — Clasificación stock para venta o compra personal

- **Tema:** Compras
- **Decisión:** Clasificación stock para venta o compra personal.
- **Evidencia:** La primera vez SANIA pregunta; después recuerda la decisión por producto.
- **Alcance:** Solo stock para venta entra en inventario y anuncios.
- **Planos afectados:** Pedidos, productos, configuración.

### D-LIVE-023 — Corrección manual de clasificación

- **Tema:** Compras/configuración
- **Decisión:** Corrección manual de clasificación.
- **Evidencia:** Debe existir una opción en SANIA para cambiar la clasificación aprendida sin preguntar en cada compra.
- **Alcance:** No se definió interfaz.
- **Planos afectados:** Productos, configuración.

### D-LIVE-024 — Variantes reutilizables con límites configurables

- **Tema:** Evolución/anuncios
- **Decisión:** Variantes reutilizables con límites configurables.
- **Evidencia:** Propuesta aceptada: 10 variantes iniciales, cada una hasta 3 usos; al agotarse, generar 10 nuevas.
- **Alcance:** Es evolución, no debe entrar automáticamente en MVP; valores configurables.
- **Planos afectados:** Ficha de producto, contenido.

### D-LIVE-025 — Criterio de configurabilidad consultado

- **Tema:** Diseño funcional
- **Decisión:** Criterio de configurabilidad consultado.
- **Evidencia:** Cuando una función pueda razonablemente ser configurable, el agente debe preguntarlo antes de fijarla.
- **Alcance:** No significa hacer todo configurable.
- **Planos afectados:** Configuración transversal.

### D-LIVE-026 — Precio calculado por coste y dos márgenes

- **Tema:** Pricing futuro
- **Decisión:** Precio calculado por coste y dos márgenes.
- **Evidencia:** Habrá margen mínimo y margen/precio de publicación; el ejemplo de mínimo fue 25 %.
- **Alcance:** Fórmula exacta, costes y diferencias por plataforma pendientes.
- **Planos afectados:** Productos, precios, finanzas.

### D-LIVE-027 — Negociación aplazada

- **Tema:** Evolución
- **Decisión:** Negociación aplazada.
- **Evidencia:** Las ofertas y contraofertas no se trabajan hasta poder automatizar o asistir de forma válida las plataformas.
- **Alcance:** Fuera de la primera versión.
- **Planos afectados:** Backlog futuro.

### D-LIVE-028 — Sin ubicaciones de almacén por ahora

- **Tema:** Stock
- **Decisión:** Sin ubicaciones de almacén por ahora.
- **Evidencia:** El almacén actual es una caja debajo del escritorio; no se guardan ubicaciones detalladas.
- **Alcance:** Revisar si crece el volumen.
- **Planos afectados:** Unidades, almacén.

### D-LIVE-029 — Mejora de anuncios a partir de dudas reales

- **Tema:** Anuncios
- **Decisión:** Mejora de anuncios a partir de dudas reales.
- **Evidencia:** Las preguntas repetidas se convierten en información de la descripción; fotos y texto se ajustan si el anuncio rinde poco.
- **Alcance:** Proceso actual manual; medición objetiva pendiente.
- **Planos afectados:** Contenido de anuncios.

### D-LIVE-030 — Importe de venta se registra sin cálculo manual inmediato

- **Tema:** Finanzas
- **Decisión:** Importe de venta se registra sin cálculo manual inmediato.
- **Evidencia:** El precio del correo se guarda; los cálculos de beneficio se harán en la parte de finanzas.
- **Alcance:** No cerrar beneficio hasta costes reales.
- **Planos afectados:** Ventas, finanzas.

## 4. Casos reales

### E-LIVE-001 — Venta y envío ordinarios

- **Situación inicial:** Venta recibida por correo.
- **Producto/plataforma:** Producto anunciado en Wallapop/Vinted.
- **Disparador:** Correo de venta.
- **Pasos:** Abrir correo/enlace o conversación; ver instrucciones; identificar transportista y QR; revisar anuncio y mensajes; localizar unidad; embalar; entregar QR; avisar al cliente; esperar entrega y OK.
- **Datos:** Título/anuncio, comprador, precio, transportista, QR, tracking, correos de admisión/entrega.
- **Resultado:** Venta cerrada solo tras el hecho final de la plataforma.
- **Rareza o dificultad:** El tracking de Wallapop puede ser difícil de identificar.
- **Comportamiento esperado de SANIA:** Registrar la venta, reservar unidad, guiar tareas físicas y no cerrar antes de tiempo.

### E-LIVE-002 — Extra o lote acordado por conversación

- **Situación inicial:** El anuncio ofrece un extra por un precio o el comprador solicita un lote.
- **Producto/plataforma:** Producto base más extra/producto de stock.
- **Disparador:** Petición explícita del comprador.
- **Pasos:** Pedir oferta que sume el extra; modificar el anuncio para que refleje el contenido; aceptar y enviar lo descrito finalmente.
- **Datos:** Precio base, precio extra, descripción final, unidades incluidas.
- **Resultado:** La venta queda documentada en el anuncio modificado.
- **Rareza o dificultad:** No se aportó correo real del caso.
- **Comportamiento esperado de SANIA:** Mantener relación entre venta y todas las unidades/productos incluidos.

### E-LIVE-003 — Última unidad anunciada en dos plataformas

- **Situación inicial:** Solo queda una unidad y existen anuncios en Wallapop y Vinted.
- **Producto/plataforma:** Última unidad compatible.
- **Disparador:** Primer correo de venta.
- **Pasos:** Reservar unidad; avisar por Telegram; Víctor elimina inmediatamente el otro anuncio.
- **Datos:** Referencia de unidad, plataformas publicadas, correo de venta.
- **Resultado:** Evitar doble venta.
- **Rareza o dificultad:** Dos correos simultáneos no están resueltos.
- **Comportamiento esperado de SANIA:** Bloquear unidad de forma atómica y crear tarea de retirada.

### E-LIVE-004 — Pedido AliExpress dividido/consolidado

- **Situación inicial:** Un pedido grande contiene productos de distintos vendedores.
- **Producto/plataforma:** Varios productos y paquetes.
- **Disparador:** Llegadas parciales y tracking.
- **Pasos:** Los paquetes llegan a cuentagotas; se comprueban cantidades y productos; la recepción final exige tenerlos en mano y verificar concordancia.
- **Datos:** Pedido, vendedor, paquete, tracking, cantidades.
- **Resultado:** Solo lo físicamente confirmado entra en stock.
- **Rareza o dificultad:** Puede haber una matrioska de productos de vendedores distintos.
- **Comportamiento esperado de SANIA:** Mantener pedido abierto y paquetes independientes hasta confirmación.

### E-LIVE-005 — Corrección manual de stock

- **Situación inicial:** Víctor detecta un fallo en el Excel.
- **Producto/plataforma:** Cualquier producto.
- **Disparador:** Recuento o error observado.
- **Pasos:** Abrir Excel y cambiar el dato según el fallo.
- **Datos:** Cantidad anterior/nueva no aportada.
- **Resultado:** Stock corregido manualmente.
- **Rareza o dificultad:** No existe hoy un flujo auditable completo.
- **Comportamiento esperado de SANIA:** Crear corrección con antes/después, actor, fecha y motivo.

### E-LIVE-006 — Optimización manual del anuncio

- **Situación inicial:** Hay dudas repetidas o poco interés.
- **Producto/plataforma:** Anuncios existentes.
- **Disparador:** Conversaciones acumuladas o pocos clics.
- **Pasos:** Analizar conversaciones con ChatGPT; añadir respuestas frecuentes; cambiar fotos o descripción.
- **Datos:** Preguntas frecuentes y señales de rendimiento.
- **Resultado:** Menos fricción y anuncio revisado.
- **Rareza o dificultad:** No hay métrica ni umbral definidos.
- **Comportamiento esperado de SANIA:** Conservar versiones y motivo del cambio, sin automatizar plataforma.

### E-LIVE-007 — Cierre final de Vinted

- **Situación inicial:** La venta ya fue entregada y finaliza.
- **Producto/plataforma:** Ejemplos: Ventilador anti vaho; Airsoft face rack + balaclava.
- **Disparador:** Correo La transacción se ha completado / TX-COMPLETE.
- **Pasos:** Extraer número de transacción, fecha, título, precio de artículo, envío y transferencia al saldo.
- **Datos:** Ejemplos: 17,00 € + 2,65 €; 25,00 € + 3,85 €; 24,95 € + 4,99 €.
- **Resultado:** Venta cerrada y saldo transferido.
- **Rareza o dificultad:** Dos ventas con mismo título pero importes distintos.
- **Comportamiento esperado de SANIA:** Cerrar Vinted idempotentemente por número de transacción.

### E-LIVE-008 — Entrega de Wallapop aún no cerrada

- **Situación inicial:** El transportista entrega el paquete al comprador.
- **Producto/plataforma:** Gafas balísticas + máscara airsoft.
- **Disparador:** Correo Wallapop Envíos: tu paquete ha sido entregado :).
- **Pasos:** Registrar entrega y mantener venta abierta hasta OK del comprador.
- **Datos:** Precio 50,00 €, fecha de compra 23/7/26, comprador abreviado.
- **Resultado:** Pendiente de disponibilidad en monedero.
- **Rareza o dificultad:** No existe correo posterior de cierre.
- **Comportamiento esperado de SANIA:** No cerrar ni calcular beneficio final con este correo.

### E-LIVE-009 — Reconocimiento económico en monedero Wallapop

- **Situación inicial:** El comprador ha confirmado y aparece entrada en el historial.
- **Producto/plataforma:** Gafas balísticas + máscara airsoft.
- **Disparador:** Movimiento Venta en el monedero.
- **Pasos:** Identificar título, imagen, fecha y 50,00 €; distinguir de retiradas, recargas y reembolsos.
- **Datos:** Historial muestra saldo actual y movimientos.
- **Resultado:** El dinero puede asociarse al anuncio.
- **Rareza o dificultad:** El mismo título de consola aparece en movimientos distintos; el título base no es identificador único.
- **Comportamiento esperado de SANIA:** Requiere confirmación manual o fuente de lectura permitida todavía no definida.

## 5. Reglas

### G-LIVE-001

- **Condición:** Un hecho externo reconocido se aplica una sola vez.
- **Resultado:** No duplicar movimientos ni reservas.
- **Estado de confirmación:** Confirmada por contexto; sin caso real de duplicado.
- **Posible contradicción o falta:** Falta probar correos repetidos y fuera de orden.

### G-LIVE-002

- **Condición:** El tracking entregado no equivale a stock disponible.
- **Resultado:** Esperar comprobación física.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Ninguna.

### G-LIVE-003

- **Condición:** Todo correcto significa producto y cantidades comprobados.
- **Resultado:** Registrar unidades y lanzar tareas de anuncio.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Definir corrección posterior.

### G-LIVE-004

- **Condición:** No OK/Abrir disputa bloquea la entrada en stock.
- **Resultado:** Crear incidencia y no inventar unidades.
- **Estado de confirmación:** Confirmada en intención.
- **Posible contradicción o falta:** Flujo de disputa pendiente.

### G-LIVE-005

- **Condición:** Cada anuncio representa una unidad.
- **Resultado:** Vinculación directa anuncio-unidad.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Compatibilidad con cantidades múltiples por anuncio no aplica.

### G-LIVE-006

- **Condición:** La referencia de unidad aparece como sufijo de tres caracteres.
- **Resultado:** Vincular correo y unidad.
- **Estado de confirmación:** Confirmada, sustituyendo regla anterior.
- **Posible contradicción o falta:** Alfabeto/colisiones pendientes.

### G-LIVE-007

- **Condición:** Primero se reserva la unidad exacta del anuncio.
- **Resultado:** Evitar FIFO salvo imposibilidad.
- **Estado de confirmación:** Confirmada por contexto y nueva referencia.
- **Posible contradicción o falta:** Diferenciación física contradictoria.

### G-LIVE-008

- **Condición:** Si una unidad está anunciada en dos plataformas, el primer correo gana.
- **Resultado:** Reservar y avisar retirada del otro anuncio.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Concurrencia simultánea pendiente.

### G-LIVE-009

- **Condición:** SANIA no crea, edita, elimina ni reactiva anuncios en plataformas.
- **Resultado:** Víctor ejecuta toda acción.
- **Estado de confirmación:** Confirmada después de una corrección.
- **Posible contradicción o falta:** Definir si existe lectura pública permitida.

### G-LIVE-010

- **Condición:** El contenido final del paquete debe coincidir con la descripción final del anuncio.
- **Resultado:** Extras solo tras modificar el anuncio.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Ninguna.

### G-LIVE-011

- **Condición:** Vinted cierra con TX-COMPLETE.
- **Resultado:** Cerrar venta y registrar transferencia.
- **Estado de confirmación:** Sustentada por tres correos reales.
- **Posible contradicción o falta:** Variantes de idioma/plantilla.

### G-LIVE-012

- **Condición:** Wallapop paquete entregado no cierra.
- **Resultado:** Mantener venta abierta.
- **Estado de confirmación:** Sustentada por correo real.
- **Posible contradicción o falta:** Captura del hecho final pendiente.

### G-LIVE-013

- **Condición:** Los avisos de publicación son independientes por plataforma.
- **Resultado:** Estados separados.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Granularidad producto/unidad pendiente.

### G-LIVE-014

- **Condición:** Las imágenes se envían solo bajo petición.
- **Resultado:** Reducir ruido en Telegram.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Formato y cantidad pendientes.

### G-LIVE-015

- **Condición:** Anuncio creado actualiza el estado de publicación.
- **Resultado:** Persistir plataforma y unidad.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Corrección si se pulsa por error.

### G-LIVE-016

- **Condición:** Las compras personales no entran en stock de venta.
- **Resultado:** Clasificación por producto.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Tratamiento de pedido mixto pendiente.

### G-LIVE-017

- **Condición:** La clasificación aprendida se puede cambiar manualmente.
- **Resultado:** Reaplicar a futuras compras.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Efecto sobre pedidos ya existentes.

### G-LIVE-018

- **Condición:** No se asigna ubicación detallada en el almacén actual.
- **Resultado:** Solo estado de stock.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Revisar al crecer.

### G-LIVE-019

- **Condición:** El precio se deriva del coste y márgenes, no de cálculo manual por venta.
- **Resultado:** Proponer precio de publicación y mínimo.
- **Estado de confirmación:** Confirmada como evolución.
- **Posible contradicción o falta:** Fórmula y costes pendientes.

### G-LIVE-020

- **Condición:** La configurabilidad se decide caso a caso.
- **Resultado:** El agente pregunta cuando tenga sentido.
- **Estado de confirmación:** Confirmada.
- **Posible contradicción o falta:** Criterios operativos por concretar.

## 6. Datos y vocabulario

| Dato/término | Finalidad | Origen | Momento | Obligatorio | Visibilidad/conservación |
|---|---|---|---|---|---|
| Referencia de unidad de 3 caracteres | Identificar unidad física y vincular correos/anuncios | Generada por SANIA | Alta de unidad / título de anuncio | Sí para unidades anunciadas | Pública en el título; histórica internamente. Colisiones pendientes. |
| Título del anuncio | Reconocer producto y contener referencia | Ficha/variante de contenido | Publicación y correos | Sí | Público; conservar versión usada. |
| Descripción final | Fuente operativa de lo que se envía | Anuncio modificado por Víctor | Antes de aceptar una venta con extras | Sí | Pública; conservar para auditoría de la venta. |
| Plataforma publicada | Saber dónde existe un anuncio | Botón Anuncio creado | Después de publicación manual | Sí | Interna; histórica. |
| Estado de publicación | Pendiente, publicada, retirada, cancelada | Telegram y confirmación humana | Durante ciclo del anuncio | Sí | Interna y auditable. |
| Correo de venta | Reservar unidad y crear tareas | Gmail | Primer correo reconocido | Sí | Conservar identificadores y extracción; evitar duplicados. |
| Transportista y QR | Entregar paquete | App/web de plataforma | Preparación de envío | Sí para envío | QR sensible y temporal; no se definió retención. |
| Tracking | Consultar estado e incidencias | App/correo transportista | Tras admisión | Sí cuando exista | Interno; relación con envío. |
| Número de transacción Vinted | Idempotencia y cierre | Correo TX-COMPLETE | Cierre | Sí | Interno y duradero. |
| Precio del artículo/venta | Movimiento económico | Correos de plataforma | Inicio y cierre | Sí | Interno; no equivale por sí solo a beneficio. |
| Precio de envío | Evidencia económica | Correo final Vinted | Cierre | Cuando se informe | Interno; tratamiento contable pendiente. |
| Movimiento del monedero Wallapop | Prueba de entrada/salida/reembolso | App/web; captura manual | Después del OK | Sí para cierre Wallapop | Fuente aún no conectada a SANIA. |
| Clasificación Stock para venta / Compra personal | Filtrar pedidos AliExpress | Pregunta inicial y configuración por producto | Compra/pedido | Sí | Interna y editable. |
| Confirmación Todo correcto | Autorizar entrada en stock | Telegram por Víctor | Tras entrega física | Sí | Auditable; debe poder corregirse. |
| Incidencia/disputa | Bloquear stock y seguimiento normal | Telegram/correo | Paquete no correcto | Según caso | Interna; estados pendientes. |
| Variante de anuncio | Rotar contenido de una ficha | Backlog de generación | Antes de publicación | No en MVP | Guardar número de usos y versión; sujeto a normas de plataforma. |
| Margen mínimo | Límite económico inferior | Configuración futura | Cálculo de precio | No definido para MVP | Interno; ejemplo 25 %, no regla fija. |
| Margen de publicación | Calcular precio mostrado | Configuración futura | Preparación del anuncio | No definido para MVP | Interno; plataforma y fórmula pendientes. |

### Vocabulario especial

- **Producto:** concepto comercial reutilizable.
- **Unidad física:** ejemplar concreto en stock; recibe la referencia de tres caracteres.
- **Anuncio:** publicación en una plataforma; en la decisión nueva representa una unidad.
- **Ficha maestra / variante:** Víctor afirma que la ficha maestra ya existe; durante la conversación llamó también “fichas” a las variaciones de contenido. La terminología debe normalizarse.
- **Todo correcto:** confirmación física de contenido y cantidades; no es solo confirmación del tracking.
- **No volver a recordar:** silencia recordatorios, pero no se ha decidido si cierra, pausa o mantiene visible la tarea.
- **Cancelar sugerencia:** descarta la tarea de publicación; no se ha definido si cambia stock o solo la intención de anunciar.

## 7. Estados y transiciones

| Entidad | Origen | Disparador | Destino | Fallo/bloqueo | Corrección auditable |
|---|---|---|---|---|---|
| Compra/producto | Detectado en AliExpress | Primera detección | Pendiente de clasificar | No se sabe si es personal o venta | Cambio manual de clasificación. |
| Compra/producto | Clasificación previa | Producto ya conocido | Stock para venta o Compra personal | Excepción de uso distinto | Opción manual para cambiar la regla. |
| Paquete | En tránsito | Tracking entregado | Pendiente de comprobación física | No respuesta | Recordatorios; No volver a recordar. |
| Paquete | Pendiente de comprobación | Todo correcto | Recibido correcto | Confirmación errónea | Flujo de corrección pendiente. |
| Paquete | Pendiente de comprobación | Abrir disputa / No OK | Incidencia | Contenido/cantidad incorrectos | Resolver disputa; estados pendientes. |
| Unidad | No existente en stock | Paquete correcto | Disponible | Nunca crear por tracking solo | Revertir con ajuste auditable. |
| Tarea de anuncio | Unidad disponible | Generación de mensaje | Pendiente de publicar | Víctor lo aplaza | Recordar más tarde / cancelar sugerencia. |
| Anuncio | Pendiente | Anuncio creado | Publicado en plataforma | Botón pulsado por error | Corrección manual pendiente. |
| Unidad | Disponible/publicada | Primer correo de venta | Reservada | Dos correos simultáneos | Conciliación pendiente; no stock negativo. |
| Anuncio alternativo | Publicado | Venta de unidad compartida | Pendiente de retirada | Víctor no retira | Recordatorio y prueba pendientes. |
| Envío | Preparación | QR y entrega al transportista | Admitido | No llega correo o tracking | Confirmación manual y contradicción pendientes. |
| Envío | Admitido | Correo/tracking de entrega | Entregado | Incidencia o tracking parado | Contactar transportista y avisar comprador. |
| Venta Vinted | Entregada/abierta | TX-COMPLETE | Cerrada | Correo duplicado | Idempotencia por número de transacción. |
| Venta Wallapop | Entregada/abierta | Movimiento de venta en monedero | Cerrada | SANIA no puede observarlo por email | Confirmación manual o lectura adicional por decidir. |
| Venta | Abierta | Cancelación | Cancelada / unidad liberable | No hay caso real | Avisar acciones manuales; regla pendiente. |
| Unidad devuelta | Devuelta | Inspección correcta | Disponible | Dañada o incompleta | Hipótesis futura, no MVP. |

## 8. Personas, permisos, avisos y tiempos

- **Usuario inicial:** Víctor.
- **Acciones humanas:** comprobar físicamente paquetes; crear, modificar, retirar y reactivar anuncios; preparar y entregar paquetes; consultar el monedero; contactar al transportista y comprador.
- **Acciones de SANIA:** leer correos, extraer/relacionar datos, reservar stock, preparar texto, generar tareas, registrar confirmaciones y recordatorios. No debe escribir en Wallapop/Vinted.
- **Canal de avisos:** Telegram, con botoneras específicas. El texto libre del contexto base no quedó derogado de forma válida.
- **Botones confirmados para publicación:** Enviar imágenes, Anuncio creado, Recordar más tarde, Cancelar sugerencia.
- **Botones confirmados para recepción:** Todo correcto, Abrir disputa/No OK, No volver a recordar en recordatorios.
- **Segunda confirmación:** no definida para cambios económicos ni acciones sensibles.
- **Tiempos conocidos:** Vinted indica 5 días para enviar en el ejemplo aportado. No se fijaron cadencias internas, horas diarias ni umbrales de extravío.
- **Ausencia de respuesta:** recordatorio periódico, pero frecuencia y efecto de silenciar siguen pendientes.

## 9. Excepciones y protecciones

- No sumar stock por un estado de tracking; exigir comprobación física.
- No permitir stock negativo ni inventar una unidad cuando dos ventas compiten.
- Aplicar un correo/evento como máximo una vez.
- Mantener la venta abierta mientras el correo solo confirme entrega.
- Si el contenido recibido no cuadra, no crear stock y abrir incidencia/disputa.
- Si el tracking queda parado, esperar un periodo todavía no definido, contactar al transportista y avisar al cliente.
- Si una unidad está cruzada en dos plataformas, reservar con el primer correo y solicitar retirada manual del otro anuncio.
- Si Víctor no responde a una comprobación, recordar sin cerrar automáticamente la tarea.
- Conservar toda corrección con antes, después, actor, fecha y motivo; el procedimiento concreto sigue pendiente.
- Los correos desconocidos, contradictorios, tardíos y duplicados no tienen casos reales; deben abrir ticket o quedar sin aplicar hasta una regla confirmada.
- La rotación de imágenes/títulos/descripciones no debe implementarse para eludir detección o normas de plataforma. Cualquier evolución requiere revisión de términos y finalidad legítima.

## 10. Contradicciones

### X-LIVE-001

- **Contexto anterior:** La referencia interna era privada y no debía aparecer en anuncios.
- **Nueva afirmación:** Víctor decidió una referencia alfanumérica de 3 caracteres visible en el título y usada también internamente.
- **Evidencia:** Confirmación explícita: visible y por unidad física.
- **Confirmación de sustitución:** Sí: la nueva decisión sustituye la regla anterior.
- **Acción/pregunta necesaria:** Actualizar todos los planos y protecciones que aún afirman que la referencia es privada.

### X-LIVE-002

- **Contexto anterior:** Tras publicar, se enviaba el enlace por Telegram para vincular anuncio y unidad.
- **Nueva afirmación:** Víctor considera innecesario el enlace porque SANIA prepara el título con la referencia y el correo la recupera.
- **Evidencia:** Explicación explícita durante la creación del anuncio.
- **Confirmación de sustitución:** Parece sustituido, pero conviene confirmarlo formalmente.
- **Acción/pregunta necesaria:** ¿Se elimina por completo el enlace o se conserva opcionalmente para navegación/auditoría?

### X-LIVE-003

- **Contexto anterior:** Durante la entrevista se formuló que SANIA retiraría automáticamente el otro anuncio.
- **Nueva afirmación:** Víctor corrigió que SANIA no puede hacer nada automáticamente en Wallapop/Vinted; solo avisar.
- **Evidencia:** Corrección explícita del usuario.
- **Confirmación de sustitución:** Sí: la versión válida es aviso y acción manual.
- **Acción/pregunta necesaria:** Eliminar cualquier automatización de escritura en plataformas.

### X-LIVE-004

- **Contexto anterior:** Cada código identifica una unidad física exacta.
- **Nueva afirmación:** Las unidades idénticas no tienen diferencias notables ni etiquetas y se localizan visualmente en una caja.
- **Evidencia:** Respuesta física explícita.
- **Confirmación de sustitución:** No resuelta.
- **Acción/pregunta necesaria:** ¿Cómo se mantiene identidad de unidad si dos unidades son físicamente intercambiables?

### X-LIVE-005

- **Contexto anterior:** Se pretende automatizar el cierre de Wallapop mediante correos y referencia en título.
- **Nueva afirmación:** No existe correo final; el cierre solo se ve en app/web/monedero, y después se afirmó solo lectura de correos.
- **Evidencia:** Correo de entrega y captura del monedero.
- **Confirmación de sustitución:** No resuelta.
- **Acción/pregunta necesaria:** ¿El cierre será una confirmación manual de Víctor o se autoriza una fuente de lectura adicional?

### X-LIVE-006

- **Contexto anterior:** Devoluciones estaban fuera del MVP hasta tener un caso real.
- **Nueva afirmación:** Se respondió hipotéticamente que una devolución correcta vuelve a venta tras inspección.
- **Evidencia:** Respuesta sin caso real.
- **Confirmación de sustitución:** No: mantener como hipótesis futura.
- **Acción/pregunta necesaria:** Retomar con la primera devolución real.

### X-LIVE-007

- **Contexto anterior:** No se propone scraping ni navegador y la lectura pública estaba pendiente.
- **Nueva afirmación:** Víctor mencionó Playwright para adquirir información; después limitó SANIA a lectura de correos.
- **Evidencia:** Dos afirmaciones de la sesión.
- **Confirmación de sustitución:** No resuelta; la última limita más.
- **Acción/pregunta necesaria:** Confirmar si toda lectura web queda descartada en MVP.

### X-LIVE-008

- **Contexto anterior:** Telegram admite texto libre para contexto/tickets.
- **Nueva afirmación:** El entrevistador interpretó una respuesta ambigua como nada de texto libre, pero Víctor solo confirmó botoneras concretas.
- **Evidencia:** No hubo confirmación explícita de prohibición global.
- **Confirmación de sustitución:** No resuelta; no sobrescribir el contexto.
- **Acción/pregunta necesaria:** ¿Se mantiene texto libre además de botones?

### X-LIVE-009

- **Contexto anterior:** Pricing, conversaciones y generación avanzada son futuros.
- **Nueva afirmación:** La sesión definió márgenes, variantes e ideas de generación de contenido.
- **Evidencia:** Conversación de evolución.
- **Confirmación de sustitución:** No deben promoverse al MVP sin decisión expresa.
- **Acción/pregunta necesaria:** Separar backlog de primera versión.

### X-LIVE-010

- **Contexto anterior:** Se propuso que el título fuera identificador fiable por sí solo.
- **Nueva afirmación:** La captura muestra el mismo título de consola en dos movimientos distintos.
- **Evidencia:** Captura del monedero.
- **Confirmación de sustitución:** Resuelta mediante sufijo de referencia, no mediante título base único.
- **Acción/pregunta necesaria:** Validar que el sufijo se conserva en todos los correos/movimientos.

### X-LIVE-011

- **Contexto anterior:** Se habló de dos mensajes por producto.
- **Nueva afirmación:** También se decidió un anuncio por unidad; un producto puede tener varias unidades.
- **Evidencia:** Expresiones usadas indistintamente.
- **Confirmación de sustitución:** No resuelta.
- **Acción/pregunta necesaria:** ¿Las tareas se crean por unidad o por producto y plataforma?

### X-LIVE-012

- **Contexto anterior:** Todo debía ser configurable.
- **Nueva afirmación:** Víctor aclaró que no quiere pasarse: el agente debe preguntar cuando tenga sentido.
- **Evidencia:** Aclaración explícita.
- **Confirmación de sustitución:** Sí: criterio consultivo, no regla absoluta.
- **Acción/pregunta necesaria:** Reflejarlo como principio de diseño, no requisito universal.

## 11. Preguntas abiertas

| Prioridad | ID | Motivo | Evidencia necesaria |
|---|---|---|---|
| Alta | `T06-Q02` | Definir el hecho de cierre de Wallapop que SANIA puede recibir sin escribir ni leer directamente la plataforma. | Demostración del proceso manual o autorización de una fuente de lectura. |
| Alta | `T04-Q02 / T04-Q05` | Resolver la identidad física de unidades idénticas sin etiquetas frente a una referencia única por unidad. | Caso con dos unidades iguales y explicación de cómo se asigna la vendida. |
| Alta | `T01-Q03` | Aclarar significado y estabilidad de b, i y r en URLs de Wallapop, o declarar que dejan de usarse. | Tres enlaces reales de operaciones diferentes. |
| Alta | `T02-Q02` | Identificador estable de línea de AliExpress. | Correo/página real de un pedido con varias líneas y paquetes. |
| Alta | `T06-Q05 / Q06 / Q10` | Cerrar modelo de beneficio real y reparto de gastos. | Dos ventas reales con coste, portes, comisiones, embalaje, impuestos y correcciones. |
| Alta | `T10-Q02` | Concurrencia: dos correos de venta casi simultáneos sobre la última unidad. | Regla de prioridad y ejemplo de conciliación. |
| Alta | `T05-Q06` | Confirmación manual seguida de correo contradictorio. | Decidir prioridad y ticket. |
| Media | `T07-Q04 / Q05` | Semántica y cadencia de Recordar más tarde, No volver a recordar y Cancelar sugerencia. | Tiempos y efecto sobre el estado. |
| Media | `T01-Q05 / T10-Q03` | Datos del comprador y datos sensibles. | Lista mínima, finalidad y retención. |
| Media | `T03-Q04 / T06-Q09` | Correcciones auditables de stock y dinero. | Caso real antes/después y motivo. |
| Media | `T05-Q04` | Correos reales de admisión por Correos/InPost y relación con tracking. | Adjuntar ejemplos. |
| Media | `T09-Q01` | Confirmar si existe lectura pública sin sesión o solo Gmail. | Decisión explícita de alcance. |
| Media | `T04-Q06 / creación` | Aclarar si las tareas de anuncios se generan por producto o por cada unidad. | Ejemplo de un pedido con 3 unidades iguales. |
| Media | `T03-Q08` | Especificar alfabeto y colisiones de la referencia de 3 caracteres. | Número máximo de unidades y política de reutilización. |
| Baja | `T10-Q01-Q06` | Volumen, dispositivos, tolerancias, contingencia y acceso. | Entrevista operativa dedicada. |

## 12. Temas aplazados

- **Cancelaciones:** Retomar cuando exista una cancelación real y se pueda aportar su correo.
- **Duplicados y desorden:** Retomar con el primer caso real; mientras tanto mantener idempotencia como protección.
- **Defectos, faltantes y variante equivocada:** Retomar con el primer caso real de AliExpress.
- **Devoluciones:** No promover al MVP; retomar con correo, estado físico y dinero de la primera devolución.
- **Extravíos:** Retomar con procedimiento real por plataforma/transportista.
- **Negociación y contraofertas:** Aplazado hasta que exista una forma permitida de asistir/automatizar el canal.
- **Generación y rotación avanzada de anuncios:** Backlog: variantes de imagen, título y descripción. Requiere revisión de normas de plataforma y no debe plantearse como evasión de detección.
- **Modelo de imagen sin huella:** Idea expresada por Víctor para que Vinted no detecte contenido generado; no es una decisión funcional válida sin revisión legal y de términos de servicio.
- **Pricing completo:** Aplazado hasta definir costes, comisiones, impuestos y fórmula de margen.
- **Ubicaciones de almacén:** Retomar si se supera la caja bajo el escritorio.
- **Ficha maestra:** Víctor indicó que ya existe en el proyecto; el informe no puede verificar su contenido.

## 13. Fragmentos literales

- “La busco con mis ojos y mis manos.”
- “No tengo un almacén, tengo una caja debajo de mi escritorio.”
- “O se vende algo o no se vende.”
- “Lo tengo en las manos y lo veo y lo toco.”
- “No existe tal correo, esa confirmación ocurre en la app o en la web.”
- “En el monedero de Wallapop se ven las transacciones por anuncio.”
- “Visible, 3 caracteres.”
- “Cada unidad física.”
- “Es un anuncio por unidad.”
- “Ella no puede hacer nada automáticamente sobre Wallapop.”
- “Es para hacer copy y pega básicamente.”
- “No volver a recordar.”
- “Stock para venta o compra personal.”
- “Todo lo que pueda tener sentido configurable, pregúntamelo antes.”

## 14. Nota para Codex

### Tres cambios de planos que parecen necesarios

1. Sustituir el modelo “referencia privada + enlace por Telegram” por “referencia alfanumérica de tres caracteres visible en el título”, dejando explícita la exposición pública y su relación uno-a-uno con la unidad.
2. Modelar publicación asistida por plataforma como tareas separadas con botoneras, imágenes bajo demanda y confirmación persistente de Anuncio creado; toda escritura en Wallapop/Vinted es humana.
3. Separar entrega logística de recepción física y de cierre económico: tracking entregado no crea stock; Wallapop paquete entregado no cierra venta; Vinted TX-COMPLETE sí cierra.

### Tres zonas de mayor incertidumbre

1. Cómo cerrar Wallapop en SANIA sin correo final ni acceso definido al monedero.
2. Cómo sostener identidad exacta de unidad física si las unidades idénticas no tienen etiqueta ni rasgo diferenciador.
3. Modelo financiero completo: costes, comisiones, impuestos, reparto, correcciones y fórmula de margen.

### Archivos, correos o ejemplos que debe aportar Víctor

- Correos AliExpress de compra, línea, envío, división, entrega y reembolso.
- Correos de admisión de Correos e InPost y ejemplos de tracking con incidencia/intento fallido.
- Primer correo real de cancelación, devolución, disputa y extravío.
- Ejemplo de tres unidades idénticas y cómo se asignan referencias físicas y anuncios.
- Dos ventas completas con todos los costes para validar beneficio normal y raro.
- Volúmenes actuales y tiempos tolerables.

**Este informe es materia prima de entrevista y no modifica por sí solo los planos de SANIA**.

---

# Apéndice A — Registro cronológico de preguntas, respuestas y puntualizaciones

Este registro es funcional: conserva todas las preguntas y respuestas sustantivas de la sesión, incluidas correcciones y propuestas. No reproduce muletillas ni repeticiones idénticas causadas por problemas de escucha.

| ID | Pregunta o tema | Respuesta de Víctor | Clasificación / cobertura |
|---|---|---|---|
| `A-001` | Caso real desde correo de venta hasta unidad preparada. | Llega el correo; se entra desde el enlace o conversación; se consultan instrucciones, empresa y QR; se revisa qué se vendió; se localiza y prepara físicamente; se entrega al transportista. | T01-Q01, T05-Q01, T05-Q04 |
| `A-002` | ¿Qué se ve en las instrucciones de envío? | La empresa de transporte y un QR que identifica el paquete al entregarlo. En Vinted cree que suele ser InPost; en Wallapop se indica la empresa. | T02-Q04, T05-Q04; “Vinted siempre InPost” queda como duda, no decisión. |
| `A-003` | ¿Cómo adquiriría esa información SANIA? | Víctor sugirió Playwright; el entrevistador volvió al flujo humano. | HIPÓTESIS/CONTRADICCIÓN con límites y posterior email-only. |
| `A-004` | ¿Cómo se confirma el contenido a preparar? | Se mira el anuncio y la conversación. Sin mensajes, se envía lo descrito. Los extras acordados deben quedar reflejados modificando el anuncio. | T01-Q06-Q07, T05-Q02. |
| `A-005` | ¿Qué pasa si la conversación es ambigua? | Víctor dice que normalmente no lo es: “o se vende algo o no se vende”. | No se obtuvo caso de ambigüedad. |
| `A-006` | ¿Cómo localiza la unidad? | Con ojos y manos; está en una caja debajo del escritorio; unidades iguales no tienen diferencias notables. | T04-Q02, T05-Q01. |
| `A-007` | ¿Qué ocurre después de entregar al transportista? | Suele escribir amablemente al cliente; espera correo de entrega y OK del cliente. | T05-Q04, T06-Q03. |
| `A-008` | ¿Cómo se cierra el envío/venta? | Se espera que llegue al cliente y que el cliente dé OK; entonces se cierra. | T06-Q01-Q03. |
| `A-009` | ¿Qué hace si no llegan confirmaciones? | Mira tracking en la app; si hay incidencia espera; si no cambia, contacta al transportista y avisa al comprador. | T05-Q09, T06-Q03-Q04. |
| `A-010` | ¿Qué prueba el cierre y el importe? | Wallapop tiene correo de venta con precio; el precio se guarda. Vinted tiene correo final. Wallapop final se ve en monedero. | T06-Q01-Q02-Q07. |
| `A-011` | ¿Calcula beneficio manualmente? | No; guarda el precio y la parte de finanzas/base de datos aplicará fórmulas. | T06, D-LIVE-030. |
| `A-012` | Última cancelación real. | No recuerda ninguna. | T01-Q04-Q09 aplazadas. |
| `A-013` | Última unidad anunciada en dos plataformas. | Se detecta por almacén y Excel; se borra manualmente el otro anuncio. SANIA debe avisar por Telegram. | T01-Q08, T04-Q04. |
| `A-014` | Extra o lote por conversación. | Pide una oferta que sume el extra y modifica el anuncio para que refleje lo vendido. | T01-Q07 resuelto. |
| `A-015` | Correo duplicado o fuera de orden. | Nunca le ha pasado. | T02-Q05 aplazado. |
| `A-016` | Pedido AliExpress en varios paquetes. | Productos de vendedores distintos suelen ir separados, pero el centro logístico puede consolidarlos; los paquetes llegan a cuentagotas. | T02-Q03, T03-Q01. |
| `A-017` | ¿Cuándo está completamente recibido? | Cuando lo tiene en las manos, lo ve, lo toca y cantidades/productos concuerdan. | T03-Q01, D-LIVE-017. |
| `A-018` | Producto equivocado, defectuoso o con menos unidades. | Nunca ocurrió; abriría disputa. | HIPÓTESIS; T03-Q01/Q09. |
| `A-019` | Corrección de Excel de stock. | Va al Excel y cambia manualmente según el fallo; quiere hablar por texto de un sistema robusto. | T03-Q04, T04-Q01. |
| `A-020` | Identificación de unidades contra líneas del pedido. | Contar e identificar manualmente. | T03-Q02. |
| `A-021` | Republicar o mejorar anuncio. | Analiza conversaciones con ChatGPT, añade dudas frecuentes y cambia fotos/descripción si no tiene tirada. | T12 evolución y D-LIVE-029. |
| `A-022` | Datos que no cuadran entre Excel y plataforma. | No recuerda un caso. | Sin evidencia real. |
| `A-023` | Cambios del comprador después del trato. | Nunca pasó. | Aplazado. |
| `A-024` | Pregunta repetida de clientes. | Se convierte en información de la descripción. | Regla actual. |
| `A-025` | Correo de Wallapop “Venta confirmada”. | Incluye comprador, título, precio, total, fecha y enlace a instrucciones; no cierra la venta. | Evidencia documental. |
| `A-026` | Correo inicial de Vinted. | Incluye comprador, título, precio y plazo de 5 días; indica que el pago se transferirá cuando finalice. | Evidencia documental. |
| `A-027` | Correos finales de Vinted. | Asunto “La transacción se ha completado”, TX-COMPLETE, número, fecha y transferencia al saldo. | T06-Q01 resuelto. |
| `A-028` | Correo Wallapop paquete entregado. | Confirma entrega pero dice que el dinero estará disponible cuando el comprador confirme. | T06-Q02 parcial. |
| `A-029` | ¿Existe correo final de Wallapop? | No; la confirmación ocurre en app/web. | Puntualización crítica. |
| `A-030` | ¿Cómo se identifica el dinero en Wallapop? | En el historial del monedero se ven transacciones por anuncio y el dinero correspondiente al producto. | T06-Q02. |
| `A-031` | ¿El título debe ser único? | La captura muestra títulos repetidos. Víctor propone añadir un código corto. | Hipótesis refinada. |
| `A-032` | Longitud de referencia. | Se decide 3 caracteres alfanuméricos, visibles en el título. | D-LIVE-006. |
| `A-033` | ¿Qué identifica la referencia? | Cada unidad física. | D-LIVE-006/Q04-Q05. |
| `A-034` | ¿Un anuncio por unidad o varias? | Un anuncio por unidad. | D-LIVE-007. |
| `A-035` | Finalidad de la referencia en título. | Permitir que el correo identifique automáticamente la unidad vendida. | T01-Q02. |
| `A-036` | Publicación simultánea en plataformas. | Normalmente referencias distintas; si queda una, ambos anuncios apuntan a la misma. | D-LIVE-008. |
| `A-037` | Momento de reserva/retirada. | En cuanto llega el correo de venta. | D-LIVE-001/D-LIVE-009. |
| `A-038` | ¿Reactivar automáticamente tras cancelación? | Víctor aclara que SANIA no puede actuar en Wallapop/Vinted; debe avisar y él lo hace. | D-LIVE-010, T01-Q09. |
| `A-039` | Aviso para nuevo producto. | Debe incluir todo el texto necesario para copiar y pegar. | D-LIVE-011. |
| `A-040` | Imágenes en Telegram. | No enviarlas siempre; botón para pedirlas. | D-LIVE-012. |
| `A-041` | Botones de publicación. | Enviar imágenes, Anuncio creado, Recordar más tarde y Cancelar sugerencia. | D-LIVE-013. |
| `A-042` | ¿Cómo sabe SANIA qué anuncio se creó? | La tarea ya está ligada a una unidad/plataforma; no hace falta identificar la unidad otra vez. | D-LIVE-014. |
| `A-043` | ¿Guardar enlace? | Víctor rechaza depender del enlace; el título ya lleva la referencia. | D-LIVE-015. |
| `A-044` | ¿Una o dos tareas de plataforma? | Dos mensajes independientes, uno Wallapop y otro Vinted. | D-LIVE-016. |
| `A-045` | ¿Qué se guarda al confirmar publicación? | La referencia/unidad queda marcada como publicada en la plataforma correspondiente. | D-LIVE-014. |
| `A-046` | Seguimiento entregado de AliExpress. | Telegram pregunta por el pedido con Todo correcto o No OK/Abrir disputa; solo OK suma stock. | D-LIVE-017/018. |
| `A-047` | Falta de respuesta a recepción. | Recordatorios y botón No volver a recordar. | D-LIVE-019. |
| `A-048` | ¿Qué hace Abrir disputa? | Víctor dice que ya se habló en otro momento; no se recuperó el detalle. | PENDIENTE. |
| `A-049` | ¿Cuándo generar tareas de anuncio? | Inmediatamente al pulsar Todo correcto. | D-LIVE-020. |
| `A-050` | Pedido grande y saturación de Telegram. | Primero dos mensajes de un producto/unidad, después los dos del siguiente. | D-LIVE-021; granularidad pendiente. |
| `A-051` | Compras personales de AliExpress. | Primera vez preguntar Stock para venta o Compra personal. | D-LIVE-022. |
| `A-052` | Aprendizaje de clasificación. | Recordar por producto y aplicar automáticamente; opción manual para cambiar. | D-LIVE-022/023. |
| `A-053` | Ficha maestra. | Víctor afirma que las fichas maestras ya están en el proyecto. | No verificable con fuentes disponibles. |
| `A-054` | Variantes de ficha/contenido. | Generar 10, usar cada una 3 veces y luego crear 10 más; valores configurables. | D-LIVE-024, backlog. |
| `A-055` | Variación de imágenes/títulos/descripciones. | Idea de variar contenido y mantener siempre la referencia al final. También expresó intención de evitar detección de Vinted. | HIPÓTESIS/RIESGO; requiere revisión de normas. |
| `A-056` | Configurabilidad. | No todo obligatoriamente; el agente debe detectar cuándo tiene sentido y preguntar antes de implementar. | D-LIVE-025. |
| `A-057` | Precio del anuncio. | Calcular desde coste y margen. | D-LIVE-026. |
| `A-058` | Dos márgenes. | Margen mínimo (ejemplo 25 %) y margen/precio de venta mayor. | D-LIVE-026; fórmula pendiente. |
| `A-059` | Negociación y contraofertas. | Descartada por ahora hasta poder automatizar/asistir plataformas. | D-LIVE-027. |
| `A-060` | Ubicación física. | No: el almacén es una caja bajo el escritorio. | D-LIVE-028. |
| `A-061` | Recordatorio para preparar unidad. | Víctor respondió que no. | No crear una función genérica sin nueva evidencia. |
| `A-062` | Devolución. | Hipótesis: volver a venta tras confirmación de estado correcto; no existe caso real. | T11 aplazado; no MVP. |

# Apéndice B — Evidencias documentales aportadas

## B.1 Wallapop — venta inicial

- **Archivo:** `¡Venta confirmada! Sigue las instrucciones para enviar tu paquete .eml`
- **Asunto:** `¡Venta confirmada! Sigue las instrucciones para enviar tu paquete`
- **Campos observados:** comprador abreviado, título, precio, total, fecha de compra y enlace a instrucciones.
- **Mensaje operativo:** el dinero queda congelado y solo estará disponible tras recepción y confirmación del comprador.
- **Conclusión:** prueba inicio/reserva, no cierre.

## B.2 Wallapop — entrega

- **Archivo:** `Wallapop Envíos_ tu paquete ha sido entregado _).eml`
- **Asunto:** `Wallapop Envíos: tu paquete ha sido entregado :)`
- **Ejemplo:** producto “gafas balisticas + mascara airsoft NUEVO”, 50,00 €, compra 23/7/26.
- **Mensaje operativo:** “Cuando [comprador] confirme que todo está OK, el dinero estará disponible en tu monedero.”
- **Conclusión:** prueba entrega, no cierre.

## B.3 Wallapop — captura del monedero

- **Archivo:** `1e32de08-7514-4330-ab0a-0c6251dc0e15.png`
- **Campos visibles por movimiento:** miniatura, título o cuenta, tipo de movimiento, fecha e importe.
- **Movimientos visibles:** venta de 50,00 €, retirada de -105,00 €, recarga de 25,00 €, reembolso de 80,00 € y otro movimiento con el mismo título de consola por -80,00 €.
- **Conclusión:** el título base puede repetirse y no es identificador fiable sin el nuevo sufijo; el monedero distingue entradas, salidas, recargas y reembolsos.

## B.4 Vinted — venta inicial

- **Archivo:** `Has vendido un artículo en Vinted.eml`
- **Asunto:** `Has vendido un artículo en Vinted`
- **Campos observados:** comprador, título, precio y plazo de envío de 5 días.
- **Mensaje operativo:** el pago se transferirá cuando finalice la transacción.
- **Conclusión:** prueba inicio, no cierre.

## B.5 Vinted — cierre final

- **Archivos:** tres ejemplos de `La transacción se ha completado.eml`.
- **Código observado:** `TX-COMPLETE`.
- **Campos:** título, número de transacción, fecha, precio del artículo, precio del envío y transferencia al saldo.
- **Ejemplos económicos:**
  - Ventilador anti vaho: artículo 17,00 €, envío 2,65 €, transferencia 17,00 €.
  - Airsoft face rack + balaclava: artículo 25,00 €, envío 3,85 €, transferencia 25,00 €.
  - Mismo título en otra venta: artículo 24,95 €, envío 4,99 €, transferencia 24,95 €.
- **Conclusión:** es una plantilla final utilizable para cierre idempotente por número de transacción.

# Apéndice C — Ideas e hipótesis no confirmadas

| Idea | Problema que intenta resolver | Estado | Dependencia / riesgo |
|---|---|---|---|
| Usar Playwright para adquirir información | Acceder a datos de la app/web | Contradictoria y no confirmada | Choca con no scraping y con la decisión posterior de solo correo. |
| Generar imágenes sin “huella” | Evitar que Vinted detecte imágenes generadas o repetidas | No aprobada | Riesgo de evasión de normas; requiere revisión legal/ToS. |
| Variar título y descripción para que parezcan diferentes | Reducir repetición de anuncios | Backlog futuro | Debe mantener veracidad y no buscar eludir controles. |
| 10 variantes, 3 usos, luego 10 nuevas | Equilibrar reutilización y diversidad | Aceptada como configuración futura | Falta definición de “variante” y métricas. |
| Precio objetivo y mínimo por margen | Preparar anuncios y límites económicos | Aceptada como concepto futuro | Fórmula exacta y costes pendientes. |
| Reventa automática tras devolución correcta | Recuperar stock | Hipótesis sin caso real | Fuera de MVP; requiere inspección y evidencia. |
| Lectura del monedero Wallapop | Cerrar venta sin correo final | Necesidad abierta | Permiso/fuente no definida. |

# Apéndice D — Instrucción de actualización para el siguiente agente

1. No copie decisiones de este informe sin comprobar la sección de contradicciones.
2. Actualice primero el vocabulario y relaciones: referencia visible, anuncio por unidad y estados por plataforma.
3. Elimine de los planos cualquier acción automática de escritura en Wallapop/Vinted.
4. Mantenga cierre de Wallapop como bloqueo explícito, no como inferencia.
5. Separe primera versión de evolución: variantes generadas, pricing avanzado, negociación, devoluciones y extravíos.
6. Conserve los identificadores `Tnn-Qnn`, `D-LIVE`, `E-LIVE`, `G-LIVE` y `X-LIVE` al trasladar cambios, para mantener trazabilidad.
7. No invente detalles ausentes sobre horarios, identificadores, correos, fórmulas o estados.
