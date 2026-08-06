# Trazabilidad de la entrevista SANIA LIVE — 31/07/2026

Este documento controla el traslado íntegro del informe de entrevista a los planos activos. No sustituye la fuente: indica dónde se incorpora cada bloque, qué queda abierto y qué se conserva únicamente como evidencia o hipótesis.

## Fuente preservada

- Archivo: `INFORME-MONOLITICO-ENTREVISTA-SANIA-LIVE-2026-07-31.md`
- Longitud: 995 líneas.
- SHA-256: `8A6B1D5C85081DAAFE41B486AD0FD3977754B3E11738BC70DD0317C74C5E5829`.
- Referencia de partida declarada por el informe: commit `024c8e7`, paquete base del 29/07/2026.
- Regla de autoridad: una decisión nueva sustituye al contexto anterior solo cuando la sección de contradicciones lo confirma; lo no confirmado se conserva como pregunta, hipótesis, evidencia o evolución.

## Leyenda

- **MVP:** trasladado como comportamiento confirmado de la primera versión.
- **Pendiente:** conservado como pregunta o bloqueo; no se inventa una solución.
- **Evolución:** decisión o idea válida fuera del MVP.
- **Hipótesis:** respuesta sin caso real o propuesta no confirmada.
- **Evidencia:** caso o documento que sustenta una regla, sin convertir detalles accidentales en requisitos.

## Cobertura de preguntas `Tnn-Qnn`

| Bloque | Identificadores cubiertos | Destinos principales | Disposición |
|---|---|---|---|
| T01 · venta, reserva, extras y anuncios | `T01-Q01`, `T01-Q02`, `T01-Q03`, `T01-Q04`, `T01-Q05`, `T01-Q06`, `T01-Q07`, `T01-Q08`, `T01-Q09`, `T01-Q10`, `T01-Q11` | `crear-y-mantener-anuncios`, `registrar-venta-confirmada`, `retirar-anuncios-sin-stock`, `preparar-paquete-vendido`, mapa global | Q01/Q07/Q08 resueltas; Q02 parcial; identificadores URL, cancelación, comprador, multiunidad, medición y casos FIFO quedan pendientes o aplazados. |
| T02 · correos, paquetes e identificación externa | `T02-Q01`, `T02-Q02`, `T02-Q03`, `T02-Q04`, `T02-Q05`, `T02-Q06`, `T02-Q07`, `T02-Q08`, `T02-Q09` | `seguir-pedidos-aliexpress`, `registrar-venta-confirmada`, `entregar-paquete-al-transportista`, `seguir-envio-al-comprador`, `resolver-excepciones-operativas` | División/consolidación y protección ante formatos dudosos incorporadas; corpus, identificadores, IA y desorden siguen pendientes. |
| T03 · recepción, coste y referencia | `T03-Q01`, `T03-Q02`, `T03-Q03`, `T03-Q04`, `T03-Q05`, `T03-Q06`, `T03-Q07`, `T03-Q08`, `T03-Q09` | `confirmar-recepcion-de-pedidos`, `dar-entrada-al-almacen`, `controlar-stock-y-trazabilidad`, finanzas | Q08 resuelta; comprobación física y bloqueo de stock incorporados; cadencias, precisión, pricing y fallos reales quedan abiertos. |
| T04 · stock, identidad y concurrencia | `T04-Q01`, `T04-Q02`, `T04-Q03`, `T04-Q04`, `T04-Q05`, `T04-Q06` | `controlar-stock-y-trazabilidad`, `registrar-venta-confirmada`, `crear-y-mantener-anuncios`, mapa global | Q05/Q06 resueltas conceptualmente; identidad física sin etiquetas, correcciones y concurrencia permanecen pendientes. |
| T05 · preparación, admisión y transporte | `T05-Q01`, `T05-Q02`, `T05-Q03`, `T05-Q04`, `T05-Q05`, `T05-Q06`, `T05-Q07`, `T05-Q08`, `T05-Q09` | `preparar-paquete-vendido`, `entregar-paquete-al-transportista`, `seguir-envio-al-comprador` | Trabajo humano y obtención manual de QR incorporados; confirmaciones alternativas, contradicciones y umbrales no se fijan. |
| T06 · cierre y beneficio | `T06-Q01`, `T06-Q02`, `T06-Q03`, `T06-Q04`, `T06-Q05`, `T06-Q06`, `T06-Q07`, `T06-Q08`, `T06-Q09`, `T06-Q10` | `cerrar-venta-entregada`, `registrar-movimientos-economicos`, `calcular-beneficio-real` | Q01 resuelta con `TX-COMPLETE`; Wallapop y el modelo financiero completo quedan bloqueados o pendientes. |
| T07 · usuario, Telegram y autonomía | `T07-Q01`, `T07-Q02`, `T07-Q03`, `T07-Q04`, `T07-Q05`, `T07-Q06` | `atender-alertas-y-confirmaciones`, mapa global | Q01/Q02 resueltas; botones conocidos incorporados; segundas confirmaciones, conflictos y cadencias pendientes. |
| T08 · tickets y bloqueos | `T08-Q01`, `T08-Q02`, `T08-Q03`, `T08-Q04`, `T08-Q05` | `resolver-excepciones-operativas`, actividades que abren tickets, mapa global | Responsable inicial y bloqueos conocidos incorporados; catálogo, prioridades, plazos y prueba de retirada pendientes. |
| T09 · lectura web y anuncios publicados | `T09-Q01`, `T09-Q02`, `T09-Q03`, `T09-Q04`, `T09-Q05`, `T09-Q06` | `comprobar-productos-publicados`, `crear-y-mantener-anuncios`, mapa global | Lectura web bloqueada en MVP hasta autorización; URL deja de ser obligatoria; precios y cadencia quedan abiertos. |
| T10 · volumen, concurrencia, privacidad y continuidad | `T10-Q01`, `T10-Q02`, `T10-Q03`, `T10-Q04`, `T10-Q05`, `T10-Q06` | mapa global, `resolver-excepciones-operativas`, planes afectados | Todo el bloque sigue pendiente y visible; no se inventan métricas, permisos, tiempos ni contingencias. |
| T11 · devolución y extravío | `T11-Q01`, `T11-Q02`, `T11-Q03` | mapa global y fuera de alcance de los flujos actuales | Aplazado hasta casos reales; las respuestas hipotéticas no entran en el MVP. |

## Decisiones confirmadas `D-LIVE`

| ID | Traslado | Destino principal |
|---|---|---|
| `D-LIVE-001` | MVP: el primer correo reconocido reserva la unidad. | `registrar-venta-confirmada`, stock |
| `D-LIVE-002` | MVP: el contenido enviado coincide con la descripción final; cualquier extra debe incorporarse al anuncio. | anuncios, preparación |
| `D-LIVE-003` | MVP humano: Víctor obtiene transportista y QR desde la plataforma. | preparación, entrega |
| `D-LIVE-004` | MVP: Vinted cierra con el correo final `TX-COMPLETE`. | cierre, finanzas |
| `D-LIVE-005` | MVP/bloqueo: Wallapop no cierra por correo. | cierre, finanzas, excepciones |
| `D-LIVE-006` | MVP: referencia alfanumérica visible de tres caracteres por unidad. | mapa, stock, anuncios, ventas |
| `D-LIVE-007` | MVP: cada anuncio representa una unidad física. | anuncios, stock |
| `D-LIVE-008` | MVP: plataformas normalmente asignadas a unidades distintas; la última unidad puede compartirse. | anuncios, stock |
| `D-LIVE-009` | MVP humano: el primer correo genera retirada manual del otro anuncio. | ventas, retirada |
| `D-LIVE-010` | MVP: todas las escrituras en Wallapop/Vinted son humanas. | alcance global |
| `D-LIVE-011` | MVP: texto de anuncio listo para copiar y pegar. | creación de anuncios |
| `D-LIVE-012` | MVP: imágenes solo bajo demanda. | creación de anuncios, Telegram |
| `D-LIVE-013` | MVP: botones `Enviar imágenes`, `Anuncio creado`, `Recordar más tarde`, `Cancelar sugerencia`. | anuncios, alertas |
| `D-LIVE-014` | MVP: `Anuncio creado` persiste unidad, plataforma y estado. | anuncios, historial |
| `D-LIVE-015` | MVP: la URL no es necesaria para identificar la unidad; su conservación opcional sigue pendiente. | anuncios, ventas |
| `D-LIVE-016` | MVP: una tarea independiente por plataforma. | anuncios, Telegram |
| `D-LIVE-017` | MVP: solo la comprobación física correcta permite entrada en stock. | recepción, almacén |
| `D-LIVE-018` | MVP: `No OK/Abrir disputa` bloquea stock y abre incidencia. | recepción, excepciones |
| `D-LIVE-019` | MVP parcial: recordar la comprobación y ofrecer `No volver a recordar`; cadencia y semántica pendientes. | recepción, alertas |
| `D-LIVE-020` | MVP: tras `Todo correcto` se crea stock y se lanzan tareas de publicación sin ubicación intermedia. | almacén, anuncios |
| `D-LIVE-021` | MVP parcial: procesamiento secuencial; granularidad producto/unidad pendiente. | anuncios, alertas |
| `D-LIVE-022` | MVP: clasificación `Stock para venta` o `Compra personal`, aprendida por producto. | pedidos, almacén |
| `D-LIVE-023` | MVP: clasificación aprendida corregible manualmente. | producto, configuración |
| `D-LIVE-024` | Evolución: generar 10 variantes, usar cada una hasta 3 veces y, al agotarse las 10, generar otro lote de 10; valores configurables. | backlog de anuncios |
| `D-LIVE-025` | Principio: preguntar antes de fijar como configurable lo que razonablemente pueda serlo. | diseño transversal |
| `D-LIVE-026` | Evolución: coste y dos márgenes; 25 % solo ejemplo, fórmula pendiente. | pricing, finanzas |
| `D-LIVE-027` | Evolución aplazada: negociación y contraofertas. | backlog |
| `D-LIVE-028` | MVP: sin ubicaciones detalladas de almacén. | stock |
| `D-LIVE-029` | Proceso humano/evolución: mejorar anuncios con dudas reales y rendimiento; métricas pendientes. | anuncios |
| `D-LIVE-030` | MVP: registrar importe de venta sin calcular beneficio inmediato. | movimientos, beneficio |

## Casos reales `E-LIVE`

| ID | Uso | Destinos |
|---|---|---|
| `E-LIVE-001` | Evidencia del recorrido venta–preparación–envío–cierre y de que no debe cerrarse antes de tiempo. | venta, preparación, entrega, seguimiento, cierre |
| `E-LIVE-002` | Evidencia editorial de extra/lote reflejado en la descripción final; multiunidad aún sin ejemplo documental. | anuncios, preparación, venta |
| `E-LIVE-003` | Evidencia de última unidad compartida y retirada manual; concurrencia simultánea pendiente. | venta, stock, retirada |
| `E-LIVE-004` | Evidencia de pedidos AliExpress divididos o consolidados y recepción física parcial. | seguimiento de pedidos, recepción |
| `E-LIVE-005` | Evidencia del ajuste manual actual; el flujo auditable sigue por diseñar. | stock, excepciones |
| `E-LIVE-006` | Evidencia de mejora manual de anuncios por dudas y poco interés; métricas pendientes. | anuncios, evolución |
| `E-LIVE-007` | Evidencia de cierre Vinted con `TX-COMPLETE` y número de transacción. | cierre, movimientos, beneficio |
| `E-LIVE-008` | Evidencia de que la entrega Wallapop mantiene abierta la venta. | seguimiento, cierre |
| `E-LIVE-009` | Evidencia del movimiento final en monedero y de títulos base repetidos; fuente para SANIA pendiente. | cierre, finanzas, excepciones |

## Reglas `G-LIVE`

| ID | Disposición | Destino |
|---|---|---|
| `G-LIVE-001` | MVP: idempotencia de hechos externos. | global |
| `G-LIVE-002` | MVP: tracking entregado no equivale a stock. | recepción |
| `G-LIVE-003` | MVP: `Todo correcto` acredita productos y cantidades y lanza stock/anuncios. | recepción, almacén |
| `G-LIVE-004` | MVP: `No OK/Abrir disputa` bloquea stock; flujo interno pendiente. | recepción, excepciones |
| `G-LIVE-005` | MVP: un anuncio representa una unidad. | anuncios |
| `G-LIVE-006` | MVP: referencia como sufijo visible de tres caracteres; colisiones pendientes. | anuncios, stock |
| `G-LIVE-007` | MVP parcial: reservar la unidad exacta; FIFO no se trata como caso real confirmado. | venta, stock |
| `G-LIVE-008` | MVP: primer correo gana para la última unidad; simultaneidad pendiente. | venta, retirada |
| `G-LIVE-009` | MVP: SANIA no escribe en plataformas. | alcance global |
| `G-LIVE-010` | MVP: el paquete coincide con la descripción final. | anuncios, preparación |
| `G-LIVE-011` | MVP: Vinted cierra con `TX-COMPLETE`. | cierre |
| `G-LIVE-012` | MVP: Wallapop entregado no cierra. | cierre |
| `G-LIVE-013` | MVP: estados de publicación separados por plataforma; granularidad pendiente. | anuncios |
| `G-LIVE-014` | MVP: imágenes solo bajo petición. | anuncios, Telegram |
| `G-LIVE-015` | MVP: `Anuncio creado` actualiza publicación; corrección por error pendiente. | anuncios |
| `G-LIVE-016` | MVP: compras personales no entran en stock de venta. | pedidos, almacén |
| `G-LIVE-017` | MVP: clasificación aprendida editable; efecto retroactivo pendiente. | configuración |
| `G-LIVE-018` | MVP: sin ubicación detallada actual. | stock |
| `G-LIVE-019` | Evolución: precio derivado de coste y márgenes; fórmula pendiente. | pricing |
| `G-LIVE-020` | Principio: configurabilidad caso a caso. | diseño global |

## Contradicciones `X-LIVE`

| ID | Resolución trasladada |
|---|---|
| `X-LIVE-001` | Sustituida la referencia privada por referencia visible de tres caracteres. |
| `X-LIVE-002` | Eliminada la dependencia obligatoria de pegar URL; conservación opcional pendiente. |
| `X-LIVE-003` | Eliminada toda escritura automática en Wallapop/Vinted; solo aviso y acción humana. |
| `X-LIVE-004` | No resuelta la identidad física exacta entre unidades idénticas sin etiquetas. |
| `X-LIVE-005` | No resuelto el cierre Wallapop; queda como bloqueo explícito. |
| `X-LIVE-006` | Devolución conservada como hipótesis futura, no MVP. |
| `X-LIVE-007` | Lectura web/Playwright bloqueada hasta autorización; prevalece el límite más restrictivo para el MVP. |
| `X-LIVE-008` | El texto libre de Telegram no se elimina: aporta contexto sin modificar por sí solo stock o dinero. |
| `X-LIVE-009` | Pricing, variantes y generación avanzada separados del MVP. |
| `X-LIVE-010` | El título base no es único; la identificación depende del sufijo de referencia y aún debe validarse en correos/movimientos. |
| `X-LIVE-011` | No resuelta la granularidad producto/unidad de las tareas de publicación. |
| `X-LIVE-012` | Configurabilidad tratada como criterio consultivo, no requisito universal. |

## Registro cronológico `A-001`–`A-062`

Las 62 entradas permanecen literales en el informe archivado. Su traslado funcional se controla por grupos, manteniendo cada identificador explícito:

- Venta, preparación, transporte y cierre inicial: `A-001`, `A-002`, `A-003`, `A-004`, `A-005`, `A-006`, `A-007`, `A-008`, `A-009`, `A-010`, `A-011`, `A-012`, `A-013`, `A-014`, `A-015`. Destinos: venta, preparación, entrega, seguimiento, cierre y preguntas aplazadas.
- AliExpress, recepción, stock y mejora de anuncios: `A-016`, `A-017`, `A-018`, `A-019`, `A-020`, `A-021`, `A-022`, `A-023`, `A-024`. Destinos: seguimiento de pedidos, recepción, almacén, stock, anuncios y excepciones.
- Evidencias de correos y cierre por plataforma: `A-025`, `A-026`, `A-027`, `A-028`, `A-029`, `A-030`. Destinos: registro de venta, seguimiento, cierre y finanzas.
- Referencia visible y publicación asistida: `A-031`, `A-032`, `A-033`, `A-034`, `A-035`, `A-036`, `A-037`, `A-038`, `A-039`, `A-040`, `A-041`, `A-042`, `A-043`, `A-044`, `A-045`. Destinos: anuncios, venta, stock, retirada y Telegram.
- Recepción, recordatorios, clasificación, evolución y almacén: `A-046`, `A-047`, `A-048`, `A-049`, `A-050`, `A-051`, `A-052`, `A-053`, `A-054`, `A-055`, `A-056`, `A-057`, `A-058`, `A-059`, `A-060`. Destinos: recepción, almacén, anuncios, alertas, pricing/backlog y preguntas abiertas.
- Límites finales de la sesión: `A-061`, `A-062`. `A-061` impide inventar un recordatorio genérico de preparación; `A-062` conserva la devolución solo como hipótesis futura.

## Bloques sin identificador que también se conservan

- **Identificación y criterio de fidelidad:** fecha, actor, canales, commit base, evidencias y reglas para no convertir “nunca me ha pasado” en requisito.
- **Resumen ejecutivo:** cambio de modelo de referencia, separación de hechos y bloqueo principal de Wallapop.
- **Datos y vocabulario:** referencia, título, descripción final, plataforma/estado de publicación, correos, QR, tracking, transacción Vinted, movimientos, clasificación, confirmaciones, incidencias, variantes y márgenes.
- **Estados y transiciones:** clasificación de compra, paquete entregado/pendiente/comprobado, unidad disponible, tarea de anuncio, anuncio publicado, reserva, retirada, admisión, entrega, cierres por plataforma, cancelación y devolución hipotética.
- **Personas, permisos, avisos y tiempos:** Víctor como único usuario inicial; acciones humanas y de SANIA; botones; texto libre no derogado; cadencias y segundas confirmaciones pendientes.
- **Excepciones y protecciones:** idempotencia, no stock por tracking, no stock negativo, reserva atómica, incidencias, correcciones auditables y eventos desconocidos sin aplicar.
- **Preguntas abiertas:** las prioridades alta/media/baja se mantienen en los planos y en esta matriz; ninguna se responde por deducción.
- **Temas aplazados:** cancelaciones, duplicados reales, defectos, devoluciones, extravíos, negociación, variantes avanzadas, imágenes para eludir controles, pricing completo, ubicaciones y ficha maestra.
- **Fragmentos literales:** se conservan íntegros en la fuente archivada como apoyo semántico, no como reglas aisladas.
- **Nota para Codex:** se aplican los tres cambios principales, se conservan las tres incertidumbres y la lista de evidencias futuras solicitadas.

## Evidencias documentales del apéndice B

- **B.1 Wallapop venta inicial:** prueba inicio/reserva, no cierre.
- **B.2 Wallapop entrega:** prueba entrega, no cierre.
- **B.3 Monedero Wallapop:** muestra tipo, fecha, título/imagen e importe; títulos base repetidos y fuente no conectada.
- **B.4 Vinted venta inicial:** prueba inicio y plazo de ejemplo, no cierre.
- **B.5 Vinted cierre final:** tres ejemplos `TX-COMPLETE`, número de transacción, fecha e importes; base del cierre idempotente.

## Ideas e hipótesis del apéndice C

- Playwright/lectura web: contradictoria, no autorizada para el MVP.
- Imágenes “sin huella”: no aprobadas; no se diseña evasión de controles.
- Variación de títulos/descripciones: evolución sujeta a veracidad y normas.
- 10 variantes × 3 usos: evolución configurable, no MVP.
- Precio objetivo/mínimo por margen: evolución, fórmula y costes pendientes.
- Reventa tras devolución: hipótesis sin caso real.
- Lectura del monedero: necesidad abierta, no integración asumida.

## Instrucciones del apéndice D aplicadas

1. Contrastar todas las decisiones con las contradicciones.
2. Actualizar primero vocabulario y relaciones.
3. Eliminar escritura automática en plataformas.
4. Mantener Wallapop como bloqueo explícito.
5. Separar MVP y evolución.
6. Conservar identificadores de trazabilidad.
7. No inventar horarios, identificadores, correos, fórmulas ni estados.

## Comprobaciones de cierre

- La copia archivada debe conservar el mismo SHA-256 que la fuente de Downloads.
- Deben aparecer en esta matriz los 80 identificadores `Tnn-Qnn`, 30 `D-LIVE`, 9 `E-LIVE`, 20 `G-LIVE`, 12 `X-LIVE` y 62 `A-nnn`.
- Los JSON activos deben ser válidos y las especificaciones deben generarse exclusivamente desde ellos.
- `docs/GPT live/referencia/` permanece como fotografía histórica del estado anterior.
