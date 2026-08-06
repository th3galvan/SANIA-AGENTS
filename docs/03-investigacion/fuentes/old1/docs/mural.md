# Mural de trabajo de SANIA

## Cosecha histórica de Notion — 29/07/2026

Objetivo de esta cosecha: convertir en planos la información funcional vigente de Notion, separar el material histórico y dejar visibles los huecos que todavía necesitan entrevista.

> **Estado tras la entrevista LIVE del 31/07/2026:** esta sección conserva la fotografía y las decisiones trasladadas el 29/07, pero ya no expresa por sí sola el estado vigente. Todo conflicto se resuelve en la sección «Entrevista SANIA LIVE — 31/07/2026», que sustituye expresamente FIFO automático, lectura pública, cadencias fijas y referencia privada.

### Orden de autoridad usado

1. [Planos funcionales v0.8](https://app.notion.com/p/3ac6d012fe0781198925c7135ef7318b) y los planos locales sincronizados con esa versión.
2. [Modelo operativo de inventario, precios e incidencias v0.2](https://app.notion.com/p/3ab6d012fe0781c5bc98cf1a52b2cd55).
3. Decisiones y requisitos del 28 y 29 de julio.
4. [Simplificación del MVP del 24 de julio](https://app.notion.com/p/3a76d012fe0781cd8941ed5226c535c7) y [cierre operativo del 26 de julio](https://app.notion.com/p/3a96d012fe078166b663f5e895117ccc).
5. [Arquitectura funcional v0.6](https://app.notion.com/p/3a46d012fe0781aeb856e46c0a33ef01), solo donde no contradijo a v0.8.

La arquitectura v0.3 y las ideas anteriores al cambio de alcance se trataron como antecedentes, no como promesas actuales.

### Fuentes principales consultadas

- [Página raíz SANIA](https://app.notion.com/p/39f6d012fe078191a685e0c0182fc2dc)
- [SANIA — Requisitos](https://app.notion.com/p/32a5b52901854766b38be9dffd8ad4f2)
- [SANIA — Registro de decisiones](https://app.notion.com/p/e940ec86f5c946218e8a55c99a004ad8)
- [SANIA — Preguntas abiertas](https://app.notion.com/p/c0c28b7dd02248f5b45dd7a9e7c36539)
- [SANIA — Conversaciones y descubrimientos](https://app.notion.com/p/837fa68b21ce4d96ba7d4513cd10ec04)
- [SANIA — Trabajo](https://app.notion.com/p/acb388ff60584bc4b18067dda0501a78), 48 filas recuperadas sin paginación pendiente
- [Modelo de datos mínimo](https://app.notion.com/p/3a06d012fe07818fa262c43e05d5357b)
- [Flujo Telegram del MVP](https://app.notion.com/p/3a16d012fe07814aa46ce5611a3ae92b)

### Decisiones trasladadas el 29/07 — antecedentes, no vigencia actual completa

- Una única cuenta de Gmail de solo lectura sirve como entrada de correos.
- Los datos incompletos abren una pregunta o un ticket; SANIA no inventa valores.
- El texto libre de Telegram aporta contexto o abre un ticket, pero no modifica directamente stock ni dinero.
- Producto, pedido, línea, lote, unidad, anuncio, venta, envío, movimiento económico, ticket y evento son cosas distintas.
- Una venta reservaba primero la referencia exacta vinculada al anuncio y el plano v0.8 proponía FIFO si no estaba disponible. **LIVE sustituye el fallback automático: la falta de unidad exacta abre conciliación hasta validar un caso real.**
- Una venta sin unidad compatible abre conciliación y nunca crea stock negativo o ficticio.
- El ingreso sigue provisional hasta que la venta termina de verdad.
- Las correcciones conservan el antes, el después, el actor, la fecha y el motivo.
- Antecedente del 29/07: antes de guardar operaciones reales se pidió una copia recuperable. LIVE no volvió a tratar la contingencia (`T10-Q04`), por lo que debe reconfirmarse antes de considerarlo criterio vigente.
- Antecedentes pendientes de reconfirmación: pérdida máxima de 24 horas y recuperación inferior a 4 horas en prototipo; PostgreSQL cada 6 horas y archivos cada noche con operaciones reales.
- Las decisiones se separan en automáticas, propuestas para aprobación e intervención humana obligatoria; mientras la matriz no esté cerrada, las decisiones sensibles no se ejecutan solas.
- Se conservará el tiempo que tarda cada anuncio en venderse, pero todavía falta acordar desde qué instante empieza a contar.

### Conflictos depurados

1. **Devoluciones:** las fuentes del 24 de julio las incluían en el MVP; v0.8 las aplazó hasta observar un caso real. Se mantuvieron fuera de la primera versión.
2. **Reserva de venta:** fuentes anteriores pedían confirmación por Telegram y después FIFO; v0.8 reservaba con el primer correo, primero la unidad exacta y después FIFO. **LIVE conserva la reserva con el primer correo, pero retira FIFO como regla automática confirmada.**
3. **Lectura de páginas:** v0.8 dejó como pregunta una lectura pública limitada sin sesión. **LIVE la bloquea para el MVP hasta una autorización explícita y mantiene el alcance de correo.**
4. **Recordatorios:** el 29/07 se conservaron avisos diarios, cada 48 horas o semanales según la actividad. **LIVE aclara que las cadencias y horas concretas siguen pendientes y elimina esos valores como comportamiento vigente.**
5. **Pricing y conversaciones:** salieron del MVP. No se convirtieron en promesas actuales.
6. **Referencia pública:** v0.8 la mantuvo solo como dato interno. **LIVE sustituye esa decisión: referencia alfanumérica de tres caracteres visible al final del título.**
7. **Tiempo de venta:** se conserva la medición futura sin scraping ni acciones automáticas; LIVE acepta la referencia visible para identificar la unidad, pero mantiene pendiente el instante inicial de la métrica.

### Planos parciales creados

Se creó un plano parcial para cada actividad actual que ya figuraba “en entrevista” pero no tenía carpeta:

- seguir pedidos de AliExpress;
- confirmar la recepción;
- controlar stock y trazabilidad;
- crear y mantener anuncios;
- comprobar productos publicados;
- retirar anuncios sin stock;
- preparar el paquete;
- entregarlo al transportista;
- seguir el envío;
- cerrar la venta;
- registrar movimientos económicos;
- calcular beneficio;
- atender alertas;
- resolver excepciones.

Estos planos contienen solo hechos y decisiones ya presentes en Notion o en el mapa v0.8. No tienen pruebas finales ni se consideran especificados.

### Actividades futuras recuperadas del tablero

- Atender conversaciones y negociar con compradores: Víctor lo seguirá haciendo manualmente; una ayuda futura exigirá una vía autorizada y conversaciones reales anonimizadas.
- Analizar cuánto tardan los anuncios en venderse: se conservarán fechas y tendencias por producto y plataforma; la referencia visible podrá identificar la unidad, pero el instante inicial sigue pendiente.

No se creó todavía un plano de detalle para estas dos actividades porque siguen fuera de la primera versión.

### Huecos transversales

- Identidad y permisos del bot de Telegram.
- Respuestas ausentes, tardías, duplicadas o contradictorias.
- Lista exacta de acciones con segunda confirmación.
- Estados, prioridades, responsables y plazos de tickets.
- Corpus real de correos por plataforma y transportista.
- Volumen de correos, pedidos, unidades, ventas y decisiones.
- Concurrencia cuando llegan dos hechos sobre la misma unidad.
- Primer día sin datos.
- Espera tolerable y funcionamiento durante una caída.
- Datos delicados y quién no debe verlos.
- Dificultades de uso, dispositivos e idioma.

El 29/07 Notion aportó 24 horas de pérdida máxima, menos de 4 horas de recuperación y frecuencias de copia para operaciones reales. LIVE clasificó `T10-Q04` como sin tratar, así que esos valores quedan como antecedente pendiente de reconfirmación y no cierran la continuidad actual.

### Cierre de la cosecha del tablero de trabajo

La vista completa de `SANIA — Trabajo` devolvió 48 filas y `has_more: false`: 26 de descubrimiento, 12 de prototipo y 10 de futuro. Por estado había 10 por definir, 6 ideas, 18 listas, 4 en curso, 2 bloqueadas y 8 hechas.

La consulta SQL avanzada requiere otro plan de Notion, pero no fue necesaria para completar el inventario: la vista normal permitió recuperar todas las filas. Las tareas puramente técnicas sobre Supabase, workers, MCP, skills, contenedores o implementación de copias quedaron como material para la construcción; solo sus consecuencias comprobables para el negocio pasaron a los planos. No se modificó Notion.

## Entrevista SANIA LIVE — 31/07/2026

Objetivo de esta actualización: incorporar sin pérdidas la entrevista LIVE posterior al commit `024c8e7`, conservar las evidencias aportadas y corregir los planos del 29/07 únicamente donde la nueva información sustituyó o limitó decisiones anteriores.

### Fuente y control de integridad

- Informe original archivado en `entrevistas/2026-07-31/INFORME-MONOLITICO-ENTREVISTA-SANIA-LIVE-2026-07-31.md`.
- SHA-256: `8A6B1D5C85081DAAFE41B486AD0FD3977754B3E11738BC70DD0317C74C5E5829`.
- Matriz de cobertura: `entrevistas/2026-07-31/TRAZABILIDAD-INFORME-LIVE-2026-07-31.md`.
- Inventario controlado: 80 preguntas `Tnn-Qnn`, 30 decisiones `D-LIVE`, 9 casos `E-LIVE`, 20 reglas `G-LIVE`, 12 contradicciones `X-LIVE` y 62 entradas cronológicas `A-001`–`A-062`, además de los bloques sin identificador.

### Decisiones del 29/07 sustituidas o limitadas

1. **Referencia privada:** queda sustituida por una referencia alfanumérica de tres caracteres por unidad, visible como sufijo del título.
2. **Vinculación por URL:** el enlace deja de ser necesario para identificar la unidad. Su conservación opcional para navegación o auditoría sigue abierta.
3. **Un anuncio para existencias compatibles:** cada anuncio representa una unidad física. Normalmente las plataformas apuntan a referencias distintas; la última unidad puede aparecer en ambas.
4. **FIFO automático:** deja de figurar como respuesta confirmada cuando la referencia exacta no está disponible; sin caso real se abre conciliación.
5. **Lectura pública semanal:** queda bloqueada para el MVP hasta autorizar expresamente alguna lectura web. Prevalece el límite posterior de lectura de correos.
6. **Cadencias fijas:** se retiran como hechos confirmados los umbrales diarios, de 48 horas o semanales que la entrevista dejó sin definir.
7. **Pricing del 25 % y redondeo a ,95 €:** se conservan como antecedentes/evolución; el 25 % fue un ejemplo y no hay fórmula cerrada.
8. **Cancelaciones, defectos, sustituciones y devoluciones:** no se modelan como recorridos reales cerrados mientras no exista evidencia del primer caso.

### Flujo vigente trasladado

- El primer correo reconocido reserva la unidad exacta indicada por la referencia del título.
- Si la última unidad estaba anunciada en ambas plataformas, SANIA crea una tarea de retirada; Víctor actúa manualmente.
- SANIA presenta secuencialmente las dos acciones de plataforma de un objetivo producto/unidad antes de pasar al siguiente; cada acción confirmable resuelve unidad y plataforma, pero la granularidad de generación por producto o por unidad sigue pendiente.
- `Anuncio creado` persiste el estado de publicación; la URL no es requisito de identidad.
- El tracking entregado de AliExpress solo deja el paquete pendiente de comprobación. `Todo correcto` autoriza stock y tareas de anuncio; `No OK/Abrir disputa` bloquea stock.
- Los productos se clasifican como `Stock para venta` o `Compra personal`; SANIA recuerda la clasificación y Víctor puede corregirla.
- Víctor obtiene manualmente transportista, instrucciones y QR, prepara el paquete y realiza todas las acciones dentro de Wallapop/Vinted. El QR es sensible y temporal; retención y permisos siguen pendientes.
- Vinted cierra idempotentemente con `TX-COMPLETE` y número de transacción.
- Wallapop no cierra con el correo de entrega. El movimiento final aparece en el monedero, pero la forma de incorporarlo a SANIA sigue bloqueada.
- El importe comunicado se registra con su origen; no se declara beneficio definitivo sin cierre y costes completos.

### Bloqueos y preguntas que permanecen visibles

- Fuente válida para el cierre de Wallapop.
- Identidad física de unidades idénticas sin etiquetas.
- Alfabeto, colisiones, reutilización y agotamiento de referencias de tres caracteres.
- Identificadores estables de líneas AliExpress y significado de `b`, `i` y `r` en Wallapop.
- Concurrencia de dos ventas sobre la última unidad.
- Granularidad final de tareas de publicación por producto o unidad.
- Momento de reaparición tras aplazar, efecto adicional de cancelar después de descartar la tarea y estado de la tarea tras silenciar recordatorios; cadencias y respuestas contradictorias.
- Confirmaciones manuales permitidas, prioridad ante correos contradictorios y segundas confirmaciones sensibles.
- Modelo financiero completo, precisión, comisiones, portes, embalajes, impuestos, reparto y correcciones.
- Datos personales, permisos, volumen, continuidad, dispositivos y tiempos tolerables.

### Evolución y material no promovido al MVP

- Variantes reutilizables de anuncios: generar 10, usar cada una hasta 3 veces y, al agotarse las 10, generar otro lote de 10; ambos valores son configurables.
- Pricing por coste, margen de publicación y margen mínimo.
- Negociación y contraofertas.
- Mejora de anuncios a partir de dudas reales y métricas de rendimiento.
- Devoluciones, extravíos, sustituciones y reembolsos hasta observar casos reales.
- Cualquier generación o variación destinada a eludir detección o controles de plataforma queda expresamente fuera.

La carpeta `GPT live/referencia/` se conserva intacta como fotografía del estado base usado durante la entrevista; no se sincroniza con estos cambios.
