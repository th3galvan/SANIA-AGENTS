# Spec: SANIA

Proyecto `sania`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

SANIA es la aplicación personal de Víctor para automatizar el registro y el seguimiento de su negocio de comprar productos en AliExpress y venderlos en Wallapop y Vinted. La primera versión observará los correos transaccionales, pedirá por Telegram las confirmaciones físicas necesarias y mantendrá al día pedidos, almacén, ventas, envíos, incidencias e importes; el beneficio permanecerá provisional hasta completar y validar el modelo de costes. Víctor seguirá gestionando manualmente los anuncios, las conversaciones y el trabajo físico. Más adelante SANIA podrá ayudar en la selección y compra de productos, los anuncios, las devoluciones y el análisis comercial mediante vías verificadas, y crecer con empleados y nuevas líneas de ingresos basadas en contenidos y formación.

Cuando recibe información nueva de AliExpress, Wallapop o Vinted, Víctor necesita que SANIA actualice automáticamente los pedidos, el almacén, las ventas y los importes sustentados por evidencia, pidiéndole las confirmaciones que requieren comprobar algo físicamente y dejando provisional el beneficio mientras falten costes, para reducir las casi dos horas diarias de registro y dedicar su tiempo a consultar, decidir, preparar y enviar los paquetes.

Criterios de éxito:
- El tiempo dedicado a copiar y registrar manualmente los flujos de correo soportados disminuye de forma medible; llegar a cero registro manual de pedidos, almacén, ventas y beneficios es el objetivo una vez validados todos los modelos y excepciones.
- Todos los cambios reconocidos en plantillas reales quedan registrados una sola vez y cada venta cerrada muestra su evidencia, sus importes y si el resultado económico es provisional o definitivo.
- Ninguna entrada duplicada o fuera de orden crea dos movimientos de almacén, venta o dinero para el mismo hecho.

## El mapa de la aplicación

Catálogo completo de actividades por zona del negocio. Cada actividad tiene (o tendrá) sus propios planos en `actividades/<id>/`.

### Compra y aprovisionamiento

- [sin empezar] **seleccionar productos y enlaces de AliExpress** (`seleccionar-productos-y-enlaces`): Hoy Víctor elige manualmente qué vender; en una fase posterior SANIA podrá ayudar a descubrir y organizar productos y proveedores.
- [sin empezar] **vigilar precios y costes de compra** (`vigilar-precios-y-costes`): Fase posterior: comparar cantidades, portes, coste total, coste unitario e histórico de los enlaces seleccionados.; necesita antes: seleccionar-productos-y-enlaces
- [sin empezar] **detectar oportunidades de compra** (`detectar-oportunidades-de-compra`): Fase posterior: aplicar reglas de stock, precio y margen para avisar de una oportunidad relevante.; necesita antes: vigilar-precios-y-costes
- [sin empezar] **aprobar y realizar compras** (`aprobar-y-realizar-compras`): Víctor toma la decisión y compra; una fase posterior podrá prepararle oportunidades y pedir su aprobación antes de actuar.; necesita antes: seleccionar-productos-y-enlaces
- [en entrevista] **seguir pedidos de AliExpress** (`seguir-pedidos-aliexpress`): Primera versión: reconocer las familias observadas de compra confirmada, envío, recogida por transportista, entrega y reembolso por cancelación, y mantener pedidos, líneas y paquetes separados. tradeOrderLineId identifica la línea; orderId u o_ids identifican pedido o subpedido. Si falta tradeOrderLineId, SANIA no inventa una relación de línea. Siguen pendientes las variantes obligatorias y cómo expresan división, consolidación y recepción parcial.; necesita antes: aprobar-y-realizar-compras
- [en entrevista] **confirmar la recepción de pedidos** (`confirmar-recepcion-de-pedidos`): Primera versión: el seguimiento entregado deja el paquete pendiente de comprobación física. Telegram muestra el contenido esperado y ofrece Todo correcto o No OK. Desde el mismo pedido, Víctor puede cambiar después la clasificación a Compra personal; SANIA enumera los productos que se borrarán del stock y exige confirmación antes de retirar las unidades y eliminar automáticamente sus tareas de anuncio. Si la incidencia es parcial, las cantidades correctas continúan y solo se bloquea la parte afectada. La cadencia específica de recepción sigue pendiente.; necesita antes: seguir-pedidos-aliexpress

### Almacén

- [en entrevista] **dar entrada a las unidades recibidas** (`dar-entrada-al-almacen`): Primera versión: crear una unidad y una referencia no reutilizable por cada cantidad físicamente recibida, empezando en ZZZ y ampliando la longitud cuando se agoten las combinaciones. Si Víctor cambia después el pedido a Compra personal y confirma el aviso, las unidades generadas se borran del stock activo y las tareas de anuncio relacionadas se eliminan automáticamente. En una incidencia parcial, las unidades correctas continúan y las problemáticas quedan bloqueadas.; necesita antes: confirmar-recepcion-de-pedidos
- [en entrevista] **controlar el stock y la trazabilidad** (`controlar-stock-y-trazabilidad`): Primera versión: conocer el estado y el lote de cada unidad, su referencia visible de longitud inicial tres y sus anuncios y ventas. Una unidad retirada al cambiar su pedido a Compra personal deja de existir en el stock activo, aunque el cambio queda en el historial. Sin etiquetas físicas, ejemplares idénticos son intercambiables; con etiquetas, la referencia identifica la unidad exacta. Si dos plataformas disputan casi a la vez la última unidad, SANIA evita stock negativo y avisa para que Víctor decida.; necesita antes: dar-entrada-al-almacen
- [sin empezar] **ajustar existencias** (`ajustar-existencias`): Registrar correcciones físicas justificadas cuando el recuento no coincida con SANIA, sin borrar el historial y separadas de cualquier movimiento económico, aunque ambos puedan vincularse por el mismo hecho. Falta documentar el próximo ajuste real con antes, después, motivo y efecto económico.; necesita antes: controlar-stock-y-trazabilidad

### Anuncios y ventas

- [en entrevista] **crear y mantener anuncios** (`crear-y-mantener-anuncios`): SANIA crea una tarea y un único anuncio activo por producto y plataforma, sin duplicados. Cada producto reúne sus imágenes, descripción, precio, precio máximo y mínimo de negociación; por defecto el precio y mínimo son iguales en Wallapop y Vinted, pero Vinted puede tener un precio de publicación superior sin cambiar ese mínimo. Víctor fija libremente ese importe de Vinted por producto, sin fórmula ni recargo automático; si cambia el precio común, queda pendiente de revisión manual sin recalcularse ni generar tarea o recordatorio, SANIA avisa inmediatamente por Telegram y lo muestra en el dashboard. Enviar imágenes entrega las asociadas al producto. El formato y la cantidad se decidirán al definir su generación, con compatibilidad para Wallapop y Vinted. Con dos o más unidades asigna referencias distintas a Wallapop y Vinted; con una sola, ambos anuncios comparten referencia. Víctor crea personalmente cada anuncio y SANIA solo lo marca como publicado después de recibir su enlace, que Víctor puede corregir después desde SANIA o Telegram; al corregirlo, el anterior se elimina completamente y no queda en el historial, y la fecha original de confirmación de publicación se conserva para medir el tiempo hasta la venta. Si SANIA cuestiona un enlace, Víctor puede enviar el correcto por mensaje o pulsar El enlace es correcto; en ambos casos se acepta definitivamente y no vuelve a preguntarse. Si recibe el mismo enlace por segunda vez, avisa y pide confirmación; un sí equivale a El enlace es correcto. Tras una venta con stock crea una nueva tarea para la plataforma, salvo que Víctor haya pulsado Cancelar sugerencia: entonces deja de proponer ese producto allí hasta que pulse Volver a sugerir, que crea inmediatamente una tarea si hay stock y no existe un anuncio activo. Si Víctor publica por su cuenta mientras están desactivadas, vincula manualmente el anuncio con el producto y la referencia. Al pasar un pedido a Compra personal, SANIA borra las tareas relacionadas.; necesita antes: controlar-stock-y-trazabilidad
- [sin empezar] **atender conversaciones y negociar con compradores** (`atender-conversaciones-y-negociar`): Víctor seguirá respondiendo y negociando manualmente en Wallapop y Vinted. En una fase futura, y solo mediante una vía autorizada, SANIA podrá aprender de conversaciones reales anonimizadas para proponer respuestas con el tono de Víctor y escalar los casos ambiguos o conflictivos.; necesita antes: crear-y-mantener-anuncios
- [en entrevista] **comprobar qué productos están publicados** (`comprobar-productos-publicados`): SANIA conservará el enlace de cada anuncio. La investigación decidirá si la primera versión puede consultar públicamente su estado sin sesión de forma sencilla, fiable y permitida; la frecuencia inicial será semanal y configurable. Un CAPTCHA o bloqueo no cambia estados y avisa por Telegram; un aviso explícito de posible baneo pausa automáticamente la función hasta que Víctor la reactive.; necesita antes: crear-y-mantener-anuncios
- [sin empezar] **republicar anuncios con stock** (`republicar-anuncios`): Fase posterior: recomendar qué anuncios conviene reactivar. SANIA no reasignará ni republicará automáticamente; el comportamiento tras una cancelación permanece pendiente hasta disponer de un caso y un correo reales.; necesita antes: comprobar-productos-publicados, controlar-stock-y-trazabilidad
- [sin empezar] **analizar la competencia y los precios** (`analizar-competencia-y-precios`): Fase posterior: comparar anuncios y recomendar precios; cualquier lectura o cambio directo dependerá de una vía autorizada.; necesita antes: crear-y-mantener-anuncios
- [en entrevista] **registrar una venta confirmada** (`registrar-venta-confirmada`): Primera versión: extraer la referencia asignada al anuncio, contrastarla con la base de datos y reservarla sin elegir otra al vender. Si quedan unidades del producto, SANIA crea automáticamente una nueva tarea para la misma plataforma con otra referencia disponible. En los enlaces de Wallapop conserva juntos b, i y r solo como pista provisional. Una referencia ausente, cortada o alterada deja la venta sin aplicar al stock, crea un ticket de revisión y avisa por Telegram. Un conflicto casi simultáneo por la misma o la última unidad espera la decisión manual de Víctor.; necesita antes: controlar-stock-y-trazabilidad
- [sin empezar] **analizar cuánto tardan los anuncios en venderse** (`analizar-rendimiento-de-anuncios`): Fase posterior: medir el tiempo de venta desde que SANIA confirma la publicación al recibir y guardar el enlace del anuncio hasta su cierre y comparar tendencias por producto y plataforma. SANIA conserva ambos instantes y el texto completo del anuncio, sin automatizar acciones dentro de Wallapop o Vinted.; necesita antes: crear-y-mantener-anuncios, registrar-venta-confirmada
- [en entrevista] **retirar anuncios cuando se agota el stock** (`retirar-anuncios-sin-stock`): Cuando una venta inequívoca reserve la referencia asignada a un anuncio, Telegram pide retirar manualmente el anuncio de la otra plataforma si tenía esa misma referencia o si ya no queda stock del producto. Si dos plataformas disputan casi a la vez la misma o la última unidad, SANIA no inicia la retirada ni elige ganador hasta que Víctor decida cuál cancelar. La URL guardada podrá servir como comprobación adicional si la investigación autoriza su lectura pública.; necesita antes: registrar-venta-confirmada, comprobar-productos-publicados

### Preparación, envío y posventa

- [en entrevista] **preparar el paquete vendido** (`preparar-paquete-vendido`): Trabajo humano: Telegram facilita la venta, la referencia y el contenido final del anuncio; Víctor obtiene manualmente las instrucciones, el transportista y el QR desde la plataforma, comprueba que cualquier extra figure en la descripción final, reúne los artículos y prepara el paquete.; necesita antes: registrar-venta-confirmada
- [en entrevista] **entregar el paquete al transportista** (`entregar-paquete-al-transportista`): Víctor lleva el paquete al punto indicado y presenta manualmente el QR. SANIA solo podrá registrar la admisión desde una plantilla real aportada y validada; LIVE no incluyó correos de admisión de Correos/InPost. La confirmación manual alternativa y la prioridad ante un correo contradictorio siguen pendientes.; necesita antes: preparar-paquete-vendido
- [en entrevista] **seguir el envío al comprador** (`seguir-envio-al-comprador`): Primera versión: aplicar solo correos de plantillas reales validadas y mantener separados admisión, tránsito y entrega. LIVE aportó la plantilla de entrega de Wallapop, pero no corpus de admisión, tránsito, intento fallido o incidencia; los umbrales, recordatorios y ticket de extravío siguen pendientes.; necesita antes: entregar-paquete-al-transportista
- [en entrevista] **cerrar una venta entregada** (`cerrar-venta-entregada`): Primera versión: mantener separadas entrega y cierre económico. Vinted cierra idempotentemente con el correo TX-COMPLETE y su número de transacción; el correo de entrega de Wallapop mantiene la venta abierta y el cierre queda bloqueado hasta decidir cómo recibe SANIA el movimiento final del monedero.; necesita antes: seguir-envio-al-comprador
- [sin empezar] **gestionar una devolución** (`gestionar-devolucion`): Fase posterior: mantener inicialmente la incidencia de forma manual y diseñar la detección, el seguimiento de vuelta y la reposición cuando exista un primer caso real con sus correos y estados verificados.; necesita antes: registrar-venta-confirmada, controlar-stock-y-trazabilidad
- [sin empezar] **gestionar un paquete extraviado** (`gestionar-paquete-extraviado`): Pendiente para una fase posterior: primero hay que conocer y verificar el procedimiento real de Wallapop y Vinted.; necesita antes: seguir-envio-al-comprador

### Finanzas

- [en entrevista] **registrar costes, ingresos y gastos** (`registrar-movimientos-economicos`): Primera versión: guardar cada movimiento económico separado de los ajustes físicos de stock, aunque puedan relacionarse mediante el mismo hecho o expediente. En una venta de varias unidades iguales se conserva un único total y su imputación total entre número de referencias, sin fabricar movimientos adicionales. La fuente del monedero de Wallapop, el reparto de costes y el pricing siguen pendientes.; necesita antes: dar-entrada-al-almacen, registrar-venta-confirmada
- [en entrevista] **calcular el beneficio real** (`calcular-beneficio-real`): Primera versión: no calcular un beneficio definitivo hasta que la venta tenga un cierre verificable y estén relacionados sus costes reales. Vinted puede cerrarse con TX-COMPLETE; Wallapop permanece provisional mientras SANIA no disponga de una fuente de cierre acordada, y comisiones, impuestos y repartos siguen abiertos.; necesita antes: cerrar-venta-entregada, registrar-movimientos-economicos
- [sin empezar] **consultar los resultados del negocio** (`consultar-resultados-del-negocio`): Actividad sin empezar: consultar ventas, stock, ingresos, gastos y resultados por periodo. El dispositivo, idioma, superficie real y dificultades de uso siguen pendientes en T10-Q06, y el beneficio no será definitivo mientras falten cierre o costes.; necesita antes: calcular-beneficio-real
- [sin empezar] **importar el histórico financiero** (`importar-historico-financiero`): Fase posterior: trasladar el Excel existente cuando el modelo se haya probado con operaciones nuevas.; necesita antes: registrar-movimientos-economicos

### Dirección y control

- [en entrevista] **atender alertas y confirmaciones** (`atender-alertas-y-confirmaciones`): Primera versión: Telegram muestra decisiones, tareas físicas y excepciones ligadas a su operación. Recordar más tarde vuelve a presentar las tareas de publicación a las 18:00 del día siguiente según la hora local de SANIA. Las incidencias avisan diariamente por defecto y cada una permite fijar otra frecuencia persistente hasta cambiarla o cerrarla. Las cadencias de otros avisos, las respuestas contradictorias y las segundas confirmaciones permanecen pendientes.
- [en entrevista] **resolver excepciones operativas** (`resolver-excepciones-operativas`): Un hecho dudoso queda como revisión pendiente sin aplicar cambios; un ticket de revisión permite investigar una anomalía sin afirmar que exista una incidencia. Solo un problema operativo confirmado abre una incidencia con estado, prioridad, responsable y bloqueos. Una referencia ausente, cortada o alterada crea ticket y aviso por Telegram, pero no una incidencia automática.; necesita antes: atender-alertas-y-confirmaciones
- [sin empezar] **evaluar cajas estándar de embalaje** (`evaluar-cajas-estandar`): Fase posterior: medir tamaños, coste, espacio y tiempo ahorrado para decidir si conviene comprar cajas ya preparadas.; necesita antes: preparar-paquete-vendido

### Nuevas líneas de ingresos

- [sin empezar] **generar vídeos de afiliación de AliExpress** (`generar-videos-de-afiliacion`): Fase futura: producir y medir contenido automático que dirija a enlaces de afiliado de AliExpress.; necesita antes: seleccionar-productos-y-enlaces
- [sin empezar] **crear un canal de desarrollo con IA** (`crear-canal-de-desarrollo-con-ia`): Fase futura: aplicar la experiencia de producción automática para publicar contenido propio y construir reputación.; necesita antes: generar-videos-de-afiliacion
- [sin empezar] **vender formación y un método propio** (`vender-formacion-y-metodo`): Fase futura: convertir la experiencia y la reputación demostradas en un curso de automatización y un método de desarrollo con IA.; necesita antes: crear-canal-de-desarrollo-con-ia

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

- "pedido de compra": una compra realizada en AliExpress que puede contener varias unidades y cuyo seguimiento llega por correo
- "lote": las unidades recibidas juntas en un pedido y que comparten su coste real de compra
- "unidad": cada artículo físico trazable desde el lote en que se compró hasta su venta, devolución o ajuste
- "venta cerrada": una venta que la plataforma da por finalizada y cuyo ingreso, coste y beneficio ya pueden quedar asentados
- "confirmación física": respuesta de Víctor por Telegram que acredita algo que un correo no puede demostrar por sí solo, como haber recibido y revisado un paquete
- "producto y variante": el tipo de artículo que se vende; cada variante relevante, como color o modelo, se trata como un producto de almacén distinto
- "referencia de unidad": identificador lógico único que empieza con tres letras y también aparece como sufijo público del anuncio. Usa el alfabeto explícito Z, Y, ..., A, z, y, ..., a; comienza en ZZZ, no se reutiliza nunca y, al agotarse una longitud, añade una letra y reinicia desde el valor máximo, por ejemplo ZZZZ. Antes del etiquetado físico individual, la referencia puede satisfacerse con cualquiera de las unidades idénticas disponibles; después del etiquetado identifica una unidad física concreta
- "anuncio": publicación manual de un producto en Wallapop o Vinted. Solo puede existir un anuncio activo del mismo producto en cada plataforma. Si hay al menos dos unidades disponibles, Wallapop y Vinted reciben referencias distintas; si solo hay una, ambos anuncios comparten esa referencia. El orden de elección no importa y la venta descuenta la referencia ya asignada, sin decidirla de nuevo
- "tarea de publicación": propuesta de publicar un producto en una plataforma. Tras una venta con stock puede regenerarse automáticamente, salvo que Víctor haya pulsado Cancelar sugerencia para ese producto y plataforma. En ese caso SANIA deja de sugerir hasta que Víctor pulse Volver a sugerir, acción que crea inmediatamente una tarea si hay stock y no existe un anuncio activo allí; mientras tanto, cualquier anuncio posterior se vincula manualmente con el producto y la referencia
- "unidad reservada": unidad apartada desde que una plataforma comunica la venta; deja de contar como disponible aunque la operación todavía no haya terminado
- "revisión pendiente": correo o hecho que SANIA no entiende con seguridad; no cambia stock, pedido, anuncio, venta ni dinero y espera una aclaración humana
- "ticket de revisión": registro visible para revisar posteriormente un fallo o una anomalía. Puede crearse por una regla expresa, como una referencia ausente, cortada o alterada, sin declarar automáticamente una incidencia ni cambiar stock, pedido, anuncio, venta o dinero
- "incidencia operativa": problema operativo confirmado que requiere seguimiento; tiene estado, prioridad, responsable, recordatorios, bloqueos concretos, resolución e historial mínimo. Una revisión pendiente o un ticket de revisión no se convierten por sí solos en incidencia
- "línea de compra": parte concreta de un pedido de AliExpress asociada a un producto o variante. tradeOrderLineId es su identificador principal cuando está disponible; orderId u o_ids identifican el pedido o subpedido, pero no sustituyen a tradeOrderLineId para distinguir una línea
- "coste actual de unidad": coste atribuido hoy a una unidad para calcular el resultado sin borrar el coste de adquisición original; la precisión, el redondeo y las correcciones económicas siguen pendientes
- "situación física": condición material de una unidad separada del resultado económico; puede estar disponible o bloqueada por incidencia. Una devolución queda en incidencia hasta revisarse y una unidad defectuosa no es válida para la venta; los casos reales siguen pendientes como evidencia
- "resultado económico de compra": forma en que termina económicamente una compra o incidencia; el modelo detallado de reembolsos, sustituciones, pérdidas y reparto de costes permanece pendiente
- "precio mínimo de negociación": concepto futuro calculado desde el coste y un margen mínimo configurable; el 25 % fue solo un ejemplo y la fórmula, el redondeo, los costes incluidos y las diferencias por plataforma no están decididos
- "venta detectada": operación comunicada por Wallapop o Vinted que SANIA ha reconocido y todavía debe recorrer hasta su cierre
- "unidad exacta": registro lógico identificado por la referencia incluida en el anuncio vendido. Mientras no haya etiquetas físicas individuales puede satisfacerse con cualquier ejemplar idéntico disponible; cuando las unidades estén etiquetadas, la referencia queda unida a una unidad física concreta
- "unidad compatible": otro ejemplar disponible del mismo producto y variante. Sin etiquetas físicas los ejemplares idénticos son intercambiables; con etiquetas, si falta la unidad referenciada, SANIA avisa por Telegram y Víctor cambia manualmente el anuncio para vincular una disponible
- "FIFO": criterio anterior para elegir la unidad compatible más antigua; SANIA no lo usa para sustituir automáticamente una referencia física etiquetada ni para decidir qué venta gana un conflicto
- "pendiente de conciliación": situación en la que SANIA no puede relacionar un hecho con una operación concreta; no inventa ni altera datos y lo mantiene como revisión pendiente hasta aclararlo
- "evento auditable": registro cronológico que conserva el hecho de origen, el antes, el después, quién actuó y el resultado
- "botón ligado a tarea": acción que identifica la tarea concreta y, cuando corresponde a un anuncio, el producto, la plataforma y la referencia asignada al crearlo; Víctor no debe volver a introducir esa identidad
- "No volver a recordar": botón que silencia los recordatorios de recepción; sigue pendiente decidir si además pausa, mantiene visible o cierra la tarea y nunca equivale a Todo correcto
- "El enlace es correcto": acción con la que Víctor acepta definitivamente un enlace cuestionado para la tarea o el anuncio ligado; SANIA no vuelve a solicitar confirmación para ese enlace
- "texto libre": entrada permitida para contexto, dudas o tickets; la entrevista no derogó su uso global
- "publicado declarado": estado persistido al pulsar Anuncio creado para una tarea ligada a un producto, una plataforma y la referencia asignada al anuncio; no prueba por sí solo que el anuncio continúe visible
- "visibilidad real no verificable": estado actual del anuncio en la plataforma que SANIA no puede acreditar con las fuentes autorizadas; T08-Q05 deja pendientes tanto la evidencia como el mecanismo de comunicación
- "lectura pública": consulta automática de un perfil o anuncio sin iniciar sesión; está bloqueada y no autorizada en el MVP
- "Todo correcto": confirmación humana de que Víctor tuvo el contenido en las manos y que productos y cantidades concordaron; no es una confirmación del tracking
- "No OK / Abrir disputa": respuesta que bloquea la creación de unidades y abre una incidencia; el nombre definitivo del botón y el flujo interno de disputa siguen pendientes
- "unidad disponible": unidad de Stock para venta creada después de Todo correcto y no reservada, vendida ni bloqueada
- "ubicación": estantería, caja o posición física exacta; no se registra en la primera versión porque el stock actual se guarda en una caja bajo el escritorio
- "identidad física exacta": correspondencia entre una referencia lógica y un ejemplar material concreto; solo es necesaria cuando las unidades están etiquetadas individualmente, no para ejemplares idénticos sin etiqueta ni diferencias visibles
- "Anuncio creado": confirmación humana que persiste que el producto quedó publicado en la plataforma con la referencia que SANIA asignó al crear el anuncio
- "Stock para venta": clasificación de producto que permite crear unidades de inventario y tareas de anuncio
- "Compra personal": clasificación de producto que no cuenta como stock para venta ni genera nuevas tareas de anuncio; si un pedido ya confirmado pasa a Compra personal, SANIA avisa qué productos se borrarán del stock y solo los retira después de una confirmación expresa. En la misma operación borra automáticamente las tareas de anuncio relacionadas
- "unidad física": ejemplar concreto creado en inventario después de Todo correcto; conserva su propia referencia y puede ser la unidad asignada a un anuncio cuando SANIA lo crea
- "identidad física de unidades idénticas": las unidades visualmente idénticas, sin etiqueta ni rasgo diferenciador, son intercambiables: no hace falta demostrar qué ejemplar material corresponde a cada referencia lógica
- "corrección auditable": nuevo evento que conserva valor anterior, valor nuevo, actor, fecha y hora y motivo sin sobrescribir el historial
- "pricing": evolución futura basada conceptualmente en coste, margen mínimo y margen o precio de publicación; no existe fórmula exacta, redondeo ni valor fijo aprobados para el MVP
- "descripción final del anuncio": versión pública del anuncio que debe reflejar exactamente el contenido vendido, incluidos los extras acordados
- "entregada": estado logístico que no implica por sí solo cierre económico
- "cerrada": Vinted: correo final TX-COMPLETE; Wallapop: hecho económico del monedero cuya entrada en SANIA sigue bloqueada
- "anuncio alternativo": único anuncio activo del mismo producto en la otra plataforma; debe retirarse si tenía asignada la referencia vendida o si la reserva deja el producto sin unidades disponibles
- "pendiente de retirada": estado interno que indica que Víctor todavía debe retirar o confirmar manualmente el anuncio; no prueba su visibilidad real
- "retirada pendiente de acreditar": situación posterior a la acción manual en la que SANIA aún no puede cerrar la tarea porque T08-Q05 no definió mecanismo ni evidencia
- "pedido": compra de AliExpress que puede contener varias líneas y permanecer abierta mientras existan paquetes pendientes de comprobación física
- "paquete": unidad logística con seguimiento propio cuando esté disponible; puede contener partes de una o varias líneas por división o consolidación
- "entregado": estado logístico que abre una comprobación física y nunca equivale por sí solo a recibido correcto ni a stock disponible

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

(Pendiente.)

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

## 5. Reglas de negocio

(Ninguna registrada.)

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| línea de compra de AliExpress | tradeOrderLineId como identificador principal de línea cuando esté disponible, orderId u o_ids para identificar pedido o subpedido, sin usarlos como sustituto de tradeOrderLineId, producto, variante, cantidad y total pagado cuando estén presentes en la plantilla observada, correo y enlaces de origen, relación de línea pendiente cuando falte tradeOrderLineId, sin usar como sustituto el nombre, la variante, la posición ni el identificador del pedido | plantillas observadas durante la revisión de los últimos tres meses de Gmail; sus campos existen, pero todavía falta decidir cuáles son obligatorios en cada variante |
| plantilla de correo AliExpress | transaction@notice.aliexpress.com · Pedido [número]: pedido confirmado · número, estado, producto, variante, cantidad, total, destinatario, dirección y enlaces, transaction@notice.aliexpress.com · Pedido [número]: pedido enviado; y Paquete [tracking] listo para envío · pedido o paquete, estado, producto, variante, cantidad, total, dirección y seguimiento, transaction@notice.aliexpress.com · Pedido [número]: recogido por el transportista · pedido, recogida, producto, variante, cantidad, destino y seguimiento, transaction@notice.aliexpress.com · Paquete [tracking] entregado · tracking, entrega, pedido relacionado, producto, variante, cantidad, destino y enlace de confirmación, promotion@aliexpress.com · Reembolso por cancelación procesado · importe, moneda, método original, fecha, plazo previsto y enlace con pedido, AEBuyerServices@aliexpress.com · Se ha emitido un reembolso para el pedido [número] · variante adicional de la señal de reembolso | revisión de los últimos tres meses de Gmail. El catálogo prueba campos observados, no que sean obligatorios ni que destinatario, dirección o método de pago deban conservarse indefinidamente |
| unidad de inventario | referencia única de longitud inicial tres, generada con el alfabeto Z…A, z…a y bloqueada para siempre tras su uso, pedido, paquete y línea de origen únicamente cuando sean identificables y demostrables, clasificación Stock para venta o Compra personal, modo de identidad física: ejemplares idénticos intercambiables o unidad etiquetada individualmente, situación física, coste de adquisición original, coste actual, estado disponible, publicada, reservada, en incidencia, devuelta o no vendible, relación con tareas de publicación, anuncios, venta e incidencia | contenido físicamente recibido y confirmado; una incidencia parcial crea las unidades recibidas y bloquea solo las afectadas, mientras una incidencia de todo el pedido no crea unidades hasta disponer de mercancía válida |
| historial de la unidad | fecha y hora, actor, origen automático o manual, campo modificado, valor anterior y valor nuevo, comentario | cada acción automática o corrección hecha desde SANIA |
| producto y variante | nombre y código interno, variante relevante, clasificación aprendida Stock para venta o Compra personal y su historial de corrección, imágenes, descripción, precio común por defecto para Wallapop y Vinted, posible precio de publicación superior específico de Vinted fijado arbitrariamente por Víctor y marcado pendiente de revisión al cambiar el precio común sin crear tarea ni recordatorio, notificado inmediatamente por Telegram y mostrado en el dashboard, precio máximo y precio mínimo aceptable en negociación común como datos editoriales vigentes; el formato y la cantidad de imágenes se decidirán al definir su generación, con compatibilidad para Wallapop y Vinted, enlaces y vendedores de compra separados de la identidad del producto | correos de AliExpress y datos confirmados por Víctor |
| pedido y paquete de AliExpress | orderId u o_ids para el pedido o subpedido y tracking para el paquete cuando estén presentes, líneas relacionadas mediante tradeOrderLineId cuando el correo o enlace lo aporte, estado operativo, remitente, asunto y eventos originales de seguimiento, correos y enlaces de origen | correos transaccionales observados de AliExpress; si falta tradeOrderLineId no se atribuye el evento a una línea concreta |
| anuncio | plataforma, URL vigente e identificador observable del anuncio; al corregir la URL se elimina completamente la anterior y no se conserva en el historial, pero se mantiene la fecha original de confirmación de publicación que inicia la medición; aceptación definitiva si Víctor pulsó El enlace es correcto, producto y variante, lista de referencias de las unidades vinculadas, título, descripción y texto completo finales usados; la base de datos, no el título, conserva la relación canónica, estado independiente por plataforma: abierto, vendido, retirado o no verificable, tarea y confirmación Ya he publicado que originaron el estado, fecha y hora de alta en la base de datos, fecha y hora de publicación confirmada al recibir y guardar el enlace, que inicia la medición de tiempo, fecha y hora de cierre, venta o retirada y su motivo, fecha y resultado de la última comprobación pública válida cuando la función esté habilitada, errores, CAPTCHA, bloqueos y estado habilitado o pausado de la comprobación, frecuencia configurable de comprobación, semanal por defecto, tiempo hasta la venta calculado desde la confirmación de publicación | tarea de publicación y confirmación humana Ya he publicado con su enlace; correo de venta, confirmación humana de retirada y, solo si la investigación lo permite, lectura pública sin sesión del enlace guardado |
| venta | plataforma e identificadores externos, valores opacos b, i y r extraídos de enlaces de Wallapop cuando estén presentes, conservados juntos como pista provisional de correlación sin atribuir significado a cada letra, anuncio, título observado y lista de referencias extraídas y contrastadas con la base de datos, producto y variante, referencias reservadas, importe total y número de referencias, precio de venta imputado a cada unidad como total dividido entre referencias cuando todas sean unidades iguales, enlace de venta o anuncio compartido por las unidades relacionadas, nombre mostrado del comprador, visible para quien tenga acceso a la base de datos de producto y conservado hasta eliminar ese registro; su finalidad exacta sigue pendiente, número de transacción de Vinted cuando exista, evidencia de entrega separada de la evidencia de cierre, estado y fechas, correo de origen | correos transaccionales de Wallapop y Vinted |
| envío | venta, transportista, seguimiento, fecha límite, QR, etiqueta o enlace consultado manualmente cuando esté disponible; el QR es sensible y temporal y su retención sigue pendiente, estado y eventos de seguimiento | correos de la plataforma y del transportista y acciones físicas de Víctor; los casos admitidos de confirmación manual siguen pendientes |
| ajuste físico de stock | cantidad anterior y cantidad posterior, motivo, persona, fecha y hora, hecho o expediente que permite relacionarlo con un efecto económico sin fusionar ambos registros | corrección física confirmada por Víctor; el próximo caso real debe aportar cantidades, motivo y efecto económico |
| movimiento económico | operación relacionada, tipo, importe total, imputaciones por unidad cuando una venta agrupe varias referencias, sin crear movimientos monetarios adicionales, fecha, estado provisional o definitivo, origen, hecho o expediente relacionado con un ajuste físico, si existe, manteniendo ambos registros separados | compras, precios comunicados en correos, TX-COMPLETE de Vinted, señales observadas de reembolso AliExpress y, cuando se acuerde una fuente, movimientos del monedero Wallapop. Reembolso procesado o emitido no significa todavía abonado ni autoriza por sí solo cambios de stock |
| ticket de revisión | anomalía y entidad relacionada, origen y fecha, motivo por el que debe revisarse, estado de revisión y resolución | regla expresa o decisión de Víctor. Una referencia ausente, cortada, alterada o duplicada crea este registro y un aviso por Telegram sin abrir automáticamente una incidencia |
| incidencia operativa | tipo y estado Abierta, En curso, En espera o Cerrada, prioridad Normal, Alta o Urgente, entidad relacionada, origen y descripción, parte exacta que bloquea y partes que pueden continuar, historial mínimo con fecha, persona, cambio, valor anterior, valor nuevo y motivo, responsable, Víctor por defecto, resolución y motivo de cierre o descarte, frecuencia de recordatorio diaria por defecto o intervalo persistente elegido por el usuario, escalado automático habilitado, pausado o desactivado para esa incidencia | problema operativo confirmado por un flujo o por Víctor; una revisión pendiente o un ticket de revisión no crean por sí solos una incidencia |
| persona y rol | persona, rol, estado activo o inactivo, tipos de incidencia que puede asumir cuando exista una regla aprobada | Víctor es la única persona inicial; la asignación por rol queda como evolución hasta disponer de suficiente historial de incidencias |
| evento externo | plataforma y remitente, identificadores del mensaje y del negocio, fecha y hora, datos extraídos, copia del contenido de origen, resultado aplicado, dejado como revisión pendiente o convertido en incidencia después de confirmarse un problema | correos transaccionales y respuestas estructuradas de Telegram |

Números del negocio:

| Qué | Cuánto |
|---|---|
| usuarios iniciales | 1, Víctor |
| trabajo manual que se quiere eliminar | aproximadamente dos horas al día dedicadas al registro de pedidos, almacén, ventas y beneficios |

- Habla con **una cuenta de Gmail de solo lectura**: recibir y conservar los correos transaccionales sin modificar el buzón. Para AliExpress ya se observaron compra, envío, recogida, entrega y reembolso por cancelación; las variantes y los campos obligatorios siguen pendientes de completar
- Habla con **Telegram**: pedir comprobaciones físicas, agrupar propuestas de publicación con una confirmación por publicación y solicitud posterior del enlace, permitir corregir ese enlace, aceptar mediante El enlace es correcto un enlace cuestionado y pedir confirmación si se repite, y avisar de tickets, incidencias, referencias anómalas, duplicados y conflictos de venta que debe resolver Víctor; las incidencias se recuerdan diariamente por defecto y admiten una frecuencia persistente configurable
- Habla con **lectura pública sin sesión de enlaces de anuncios (condicionada a investigación)**: comprobar periódicamente si un anuncio sigue abierto, fue vendido o fue retirado; no se incluirá en la primera versión si resulta complicada, frágil o no permitida, y se pausará ante riesgo de baneo
- Habla con **Excel costes_aliexpress.xlsx**: servir como referencia del cálculo y del histórico actual hasta que se diseñe su importación
- Habla con **fuente de cierre de Wallapop (pendiente)**: resolver cómo llega a SANIA el movimiento final del monedero; no existe integración autorizada ni el correo de entrega puede sustituirla

## 8. Superficie de uso

### Telegram de SANIA

| Campo | Valor |
|---|---|
| Quién entra | Víctor |
| Por dónde llega | mediante Telegram; el dispositivo real, el idioma y las dificultades de uso siguen pendientes en T10-Q06 |
| Cuándo lo usa | SANIA necesitó una confirmación física, una decisión o avisar de una excepción |
| Qué ve nada más entrar | el contexto necesario para decidir, la operación afectada, el motivo y las acciones disponibles |
| Qué puede hacer | confirmar un hecho físico · resolver una decisión estructurada · pedir las imágenes de una tarea de publicación · confirmar Ya he publicado para una publicación y aportar su enlace · corregir el enlace de un anuncio · confirmar que un enlace cuestionado es correcto · confirmar el uso de un enlace repetido · aplazar o cancelar una sugerencia de publicación · abrir o completar un ticket de revisión o una incidencia · cambiar la frecuencia o pausar el escalado de una incidencia · reactivar una comprobación pública pausada por seguridad · consultar el estado de una tarea |
| Qué NO debe poder jamás | modificar datos operativos directamente mediante texto libre · dar por resuelta una excepción con impacto en stock o dinero sin la confirmación exigida · ser la única fuente donde se conserva una decisión |

### Consulta general de SANIA

| Campo | Valor |
|---|---|
| Quién entra | Víctor |
| Por dónde llega | mediante una superficie todavía por definir; móvil, ordenador de casa y ordenador de trabajo son antecedentes no confirmados por LIVE |
| Cuándo lo usa | quiso consultar pedidos, stock, ventas, envíos, tickets o resultados |
| Qué ve nada más entrar | el estado actual y la historia que explica cada cambio |
| Qué puede hacer | consultar operaciones · revisar excepciones · solicitar correcciones admitidas por el flujo y explicar el motivo · consultar resultados |
| Qué NO debe poder jamás | borrar silenciosamente el historial · editar directamente la fuente de datos · actuar automáticamente dentro de Wallapop o Vinted |

### Matriz de permisos

|  | confirmar un hecho físico | resolver una decisión estructurada | pedir las imágenes de una tarea de publicación | confirmar Ya he publicado para una publicación y aportar su enlace | corregir el enlace de un anuncio | confirmar que un enlace cuestionado es correcto | confirmar el uso de un enlace repetido | aplazar o cancelar una sugerencia de publicación | abrir o completar un ticket de revisión o una incidencia | cambiar la frecuencia o pausar el escalado de una incidencia | reactivar una comprobación pública pausada por seguridad | consultar el estado de una tarea | consultar operaciones | revisar excepciones | solicitar correcciones admitidas por el flujo y explicar el motivo | consultar resultados |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Víctor | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

### Avisos

| Quién se entera | De qué | Por dónde | Cuándo |
|---|---|---|---|
| Víctor | una confirmación, decisión o excepción necesitó su intervención | Telegram | cuando un hecho concreto requirió intervención; las tareas de publicación aplazadas reaparecen a las 18:00 del día siguiente según la hora local de SANIA, las incidencias repiten diariamente por defecto o con el intervalo persistente elegido, y las demás cadencias siguen pendientes por actividad |

### Condiciones de uso

- [T10-Q06] No se documentaron los dispositivos, el idioma ni las dificultades de uso reales; las superficies descritas son conceptuales hasta validarlas.
- [T07-Q02 / D-LIVE-010] SANIA registró hechos de correo y preparó tareas; Víctor confirmó hechos físicos y ejecutó todas las acciones dentro de Wallapop y Vinted.
- El texto libre de Telegram aportó contexto o abrió un ticket, pero no cambió por sí solo stock ni dinero.
- Los datos dudosos o incompletos dejaron una revisión pendiente sin inventar valores; solo un problema confirmado abrió una incidencia.
- Cualquier corrección conservó el valor anterior y el motivo.
- Hasta que una matriz acordada permitiera expresamente una acción automática, SANIA trató como propuesta o intervención obligatoria cualquier decisión sensible sobre compras, margen, stock, mensajes ambiguos, bloqueos de plataforma o situaciones conflictivas.

## 9. Calidad y límites

- **Q-5**: Un correo duplicado, repetido o recibido fuera de orden aplicó como máximo una vez el mismo cambio de pedido, stock, venta, envío o dinero.
- **Q-6**: El 100 % de los cambios de stock, estado y dinero conservó el hecho de origen, la fecha, el actor y el antes y el después cuando hubo una corrección.
- **Q-7**: Ningún flujo creó stock negativo ni una referencia ficticia, y ningún resultado económico se declaró definitivo sin relacionar la venta, la unidad y los costes reales respaldados por evidencia.
- **Q-8**: T10-Q04 mantiene pendiente la contingencia y recuperación. Los objetivos heredados del 29/07 —pérdida máxima de 24 horas y recuperación inferior a 4 horas en prototipo, PostgreSQL cada 6 horas y archivos cada noche con operaciones reales— se conservan como antecedente que debe reconfirmarse, no como criterio vigente demostrado por LIVE.
- **Q-9**: Toda publicación conservó en el título y en la base de datos la referencia que SANIA le asignó al crearla, y cada confirmación Ya he publicado quedó ligada a esa publicación y solicitó su enlace.
- **Q-10**: Ningún tracking entregado creó stock antes de Todo correcto y ningún correo de entrega de Wallapop cerró por sí solo la venta o convirtió el ingreso en definitivo.
- **Q-11**: Ninguna acción creó, editó, retiró, reactivó o publicó automáticamente un anuncio de Wallapop o Vinted; SANIA solo preparó información, registró hechos y pidió acciones humanas.
- **Q-12**: Ningún correo o hecho dudoso cambió stock, pedido, anuncio, venta o dinero; quedó como revisión pendiente o ticket de revisión, y solo abrió una incidencia tras confirmarse un problema operativo.
- **Q-13**: Una incidencia parcial bloqueó solo las cantidades afectadas y permitió continuar las correctas; una incidencia de todo el pedido no creó unidades hasta disponer de mercancía válida para la venta.
- **Q-14**: Un CAPTCHA, bloqueo o fallo temporal durante una comprobación pública no cambió el estado del anuncio; avisó por Telegram y un riesgo explícito de baneo pausó la función hasta reactivación de Víctor.
- **Q-15**: Toda incidencia tuvo estado, prioridad y responsable visible, avisó diariamente salvo frecuencia distinta elegida para ese caso y conservó un historial mínimo de los cambios relevantes.
- **Q-16**: Toda referencia se generó con el alfabeto Z…A, z…a, empezó en ZZZ para la primera longitud, no se reutilizó y amplió su longitud al agotarse; cualquier duplicado bloqueó la operación, creó un ticket y avisó por Telegram.
- **Q-17**: Una referencia de venta ausente, cortada o alterada no cambió stock ni abrió automáticamente una incidencia: creó un ticket de revisión y avisó a Víctor por Telegram.
- **Q-18**: Dos ventas casi simultáneas que disputaron la última unidad quedaron sin ganador automático y sin stock negativo hasta que Víctor decidió y canceló manualmente una.
- **Q-19**: Un evento AliExpress sin tradeOrderLineId no se atribuyó a una línea mediante nombre, variante, posición, orderId u o_ids; y una señal de reembolso procesado no se trató como abono ni produjo por sí sola cambios de stock.
- **Q-20**: Todo ajuste físico de stock y todo movimiento económico se guardaron como registros independientes, aunque compartieran el mismo hecho o expediente.
- **Q-21**: Cuando un correo de Wallapop aportó b, i y r, SANIA conservó los tres valores y pudo usarlos juntos como pista provisional para relacionar correos; si faltaron, cambiaron o se contradijeron, no unió automáticamente operaciones ni modificó stock, venta o dinero y dejó el caso para revisión.

## 10. Fuera de alcance

- Automatizar la atención de consultas, la negociación o el cierre dentro de Wallapop y Vinted: Víctor seguirá haciéndolo manualmente mientras no exista una vía autorizada.
- Primera versión: usar scraping o automatización de navegador para crear, editar, republicar o cambiar precios en Wallapop y Vinted.
- Inventar una correspondencia física exacta mientras las unidades idénticas no estén etiquetadas, o reasignar automáticamente un anuncio cuando sí exista una etiqueta física y falte la unidad referenciada.
- Primera versión: registrar estanterías, cajas o una ubicación física exacta de cada unidad; Víctor todavía no necesita localizar productos dentro de un almacén.
- Primera versión: abrir, negociar o cerrar automáticamente disputas dentro de AliExpress. El actor, el flujo manual y la forma en que SANIA seguirá el caso permanecen pendientes porque LIVE no recuperó ese detalle.
- Primera versión: extraer o analizar conversaciones de Wallapop o Vinted para estudiar la negociación o decidir qué unidad se vendió.
- Primera versión: leer perfiles, conversaciones, páginas privadas o monederos de Wallapop o Vinted. Solo podrá consultarse sin sesión el estado público del enlace de un anuncio si la investigación demuestra que es sencillo, fiable y permitido.
- Primera versión: automatizar devoluciones de compradores antes de observar un caso real y sus correos.
- Reasignar o republicar automáticamente anuncios después de una venta.
- Primera versión: resolver automáticamente un paquete extraviado antes de conocer y verificar el procedimiento real de cada plataforma.
- Primera versión: generar y rotar variantes de imágenes, títulos o descripciones, negociar ofertas o aplicar pricing avanzado antes de definir reglas, costes, métricas y condiciones de plataforma.
- Elegir automáticamente qué venta gana un conflicto por la plataforma o por la mención no verificada de 32 ventas en Wallapop; cualquier criterio fiscal o estratégico exige revisión jurídica o fiscal y una decisión posterior.
- Conservar como datos operativos permanentes el destinatario, la dirección o el método de pago observados en correos AliExpress sin acordar antes su finalidad, acceso y retención.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- ¿Cómo recibirá SANIA el hecho final de Wallapop: confirmación manual de Víctor o una fuente adicional de lectura expresamente autorizada?
- [T01-Q04 / T01-Q09] ¿Qué correo o hecho demuestra una cancelación real en Wallapop o Vinted y qué ocurre con la reserva, las unidades y los anuncios antes o después del envío?
- [T01-Q05] ¿Cuál es la finalidad funcional exacta de conservar el nombre mostrado del comprador y el enlace de la venta o anuncio?
- [T01-Q06] ¿Cómo se representa un anuncio con productos diferentes y cómo se reparten sus costes? En unidades iguales ya está decidido que comparten enlace y que el total se divide entre referencias.
- ¿Cómo indica Víctor a SANIA que un producto o lote pasó de unidades físicamente intercambiables a unidades etiquetadas individualmente?
- [T04-Q01] ¿Cuál será el próximo ajuste real de stock, con cantidad anterior, cantidad posterior, motivo y efecto económico?
- ¿Qué correos, estados y movimientos económicos produce la primera devolución real de un comprador?
- ¿Cómo gestionan realmente Wallapop y Vinted un paquete extraviado?
- ¿Cancelar dentro de una notificación agrupada cancela una publicación concreta o toda la propuesta, y cómo se corrige una pulsación errónea?
- ¿Con qué precisión se guardará el coste unitario cuando el total pagado no sea divisible exactamente entre la cantidad?
- ¿El precio inicial indicado por Víctor será común a Wallapop y Vinted o podrá ser distinto en cada plataforma?
- ¿Demostrará la investigación que se puede consultar el estado público de los enlaces de anuncios sin sesión de forma sencilla, fiable y permitida, distinguiendo vendido, retirado y fallo temporal?
- ¿Qué decisiones concretas serán automáticas, cuáles serán propuestas que Víctor debe aprobar y cuáles exigirán siempre intervención humana, y cuáles de ellas necesitan una segunda confirmación?
- ¿Qué debe ocurrir cuando Víctor no responde, responde tarde o da dos respuestas contradictorias?
- ¿Qué hechos concretos harán subir automáticamente la prioridad de una incidencia, al margen de la frecuencia de aviso elegida?
- Cuando exista suficiente historial, ¿qué tipos de incidencia se asignarán por defecto a cada rol?
- ¿Qué variantes reales faltan y cuáles de los remitentes, asuntos y campos observados son obligatorios o pueden faltar en cada plantilla de plataforma y transportista?
- ¿Qué correo o campo de AliExpress expresa división, consolidación o entrega parcial y cómo relaciona cada paquete con sus líneas?
- Cuando un correo AliExpress no incluye tradeOrderLineId, ¿qué fuente autorizada puede recuperarlo y cuándo debe quedar la línea sin relacionar?
- ¿Para qué tipos concretos de correo se permitirá un segundo intento con IA, qué datos podrá proponer y cuándo deberá dejar el formato como desconocido, sin aplicar o abrir un ticket?
- [T10-Q03 / T10-Q04] ¿Qué datos son delicados, quién no debe verlos, cuánto se conservan —incluidos destinatario, dirección y método de pago de AliExpress— y qué pasaría si SANIA no estuviera disponible durante medio día?
- [T10-Q01 / T10-Q02] ¿Cuántos correos, pedidos, unidades, ventas y decisiones procesa hoy el negocio y qué orden general se aplica a otros hechos simultáneos, tardíos o desordenados?
- [T10-Q05] ¿Qué tiempos de espera son tolerables para cada acción y canal?
- ¿Qué fórmula y costes intervienen en el precio y el beneficio, cómo se reparten comisiones, portes, embalajes e impuestos y qué correcciones requieren segunda confirmación?
- ¿Cuántos CAPTCHA o bloqueos repetidos deben pausar la comprobación pública y la pausa afecta a un anuncio, una plataforma o toda la función?
- ¿Qué política de retención o archivo necesitará el historial mínimo de incidencias y comprobaciones después de medir su volumen real?
- ¿Cada cuánto debe aparecer el resumen de optimizaciones no urgentes?

