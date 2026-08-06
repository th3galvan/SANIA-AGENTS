# Mural de trabajo de SANIA

## Cosecha de Notion — 29/07/2026

Objetivo de esta cosecha: convertir en planos la información funcional vigente de Notion, separar el material histórico y dejar visibles los huecos que todavía necesitan entrevista.

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

### Decisiones vigentes trasladadas

- Una única cuenta de Gmail de solo lectura sirve como entrada de correos.
- Los datos incompletos abren una pregunta o un ticket; SANIA no inventa valores.
- El texto libre de Telegram aporta contexto o abre un ticket, pero no modifica directamente stock ni dinero.
- Producto, pedido, línea, lote, unidad, anuncio, venta, envío, movimiento económico, ticket y evento son cosas distintas.
- Una venta reserva primero la referencia exacta vinculada al anuncio; FIFO solo se usa si esa referencia no está disponible.
- Una venta sin unidad compatible abre conciliación y nunca crea stock negativo o ficticio.
- El ingreso sigue provisional hasta que la venta termina de verdad.
- Las correcciones conservan el antes, el después, el actor, la fecha y el motivo.
- Antes de guardar operaciones reales debe existir una copia recuperable.
- Durante el prototipo se acepta como máximo una pérdida de 24 horas y una recuperación inferior a 4 horas; con operaciones reales, PostgreSQL se respalda al menos cada 6 horas y los archivos cada noche.
- Las decisiones se separan en automáticas, propuestas para aprobación e intervención humana obligatoria; mientras la matriz no esté cerrada, las decisiones sensibles no se ejecutan solas.
- Se conservará el tiempo que tarda cada anuncio en venderse, pero todavía falta acordar desde qué instante empieza a contar.

### Conflictos depurados

1. **Devoluciones:** las fuentes del 24 de julio las incluían en el MVP; v0.8 las aplazó hasta observar un caso real. Se mantuvieron fuera de la primera versión.
2. **Reserva de venta:** fuentes anteriores pedían confirmación por Telegram y después FIFO; v0.8 reserva con el primer correo, primero la unidad exacta y después FIFO. Se aplicó v0.8.
3. **Lectura de páginas:** una decisión anterior prohibía toda interacción automática; v0.8 permite una lectura pública limitada sin sesión. Quedó como pregunta explícita para confirmar.
4. **Recordatorios:** no existe una cadencia global. Se conservaron el aviso diario, cada 48 horas o semanal según la actividad.
5. **Pricing y conversaciones:** salieron del MVP. No se convirtieron en promesas actuales.
6. **Referencia pública:** una tarea antigua proponía escribirla en el anuncio; v0.8 lo prohíbe. Se mantuvo solo como dato interno.
7. **Tiempo de venta:** la misma tarea antigua mezclaba una métrica válida con scraping, generación automática de anuncios y una referencia interna pública. Solo se conservó la medición futura del tiempo hasta la venta.

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
- Analizar cuánto tardan los anuncios en venderse: se conservarán fechas y tendencias por producto y plataforma sin publicar referencias internas.

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

La pérdida máxima y el tiempo de recuperación dejaron de ser un hueco general: Notion ya fija 24 horas y menos de 4 horas para el prototipo, además de la frecuencia de copia para operaciones reales.

### Cierre de la cosecha del tablero de trabajo

La vista completa de `SANIA — Trabajo` devolvió 48 filas y `has_more: false`: 26 de descubrimiento, 12 de prototipo y 10 de futuro. Por estado había 10 por definir, 6 ideas, 18 listas, 4 en curso, 2 bloqueadas y 8 hechas.

La consulta SQL avanzada requiere otro plan de Notion, pero no fue necesaria para completar el inventario: la vista normal permitió recuperar todas las filas. Las tareas puramente técnicas sobre Supabase, workers, MCP, skills, contenedores o implementación de copias quedaron como material para la construcción; solo sus consecuencias comprobables para el negocio pasaron a los planos. No se modificó Notion.
