# Spec: Registrar una venta confirmada

Proyecto `sania-registrar-venta-confirmada`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

Esta actividad convierte el primer correo reconocido de venta de Wallapop o Vinted en la reserva trazable de la unidad exacta identificada por la referencia pública de tres caracteres al final del título. No aplica FIFO automáticamente, no depende de una URL ni de parámetros b, i y r no validados, y no confunde el inicio o la entrega con el cierre económico.

Cuando llegó el primer correo de venta, Víctor necesitó que SANIA identificara y reservara una sola vez la unidad exacta del anuncio, avisara de la retirada manual de cualquier anuncio alternativo de esa misma unidad y mantuviera la venta abierta hasta la evidencia final correcta de cada plataforma.

Criterios de éxito:
- Cada hecho externo reconocido se aplicó como máximo una vez.
- El primer correo reconocido reservó la unidad exacta indicada por el sufijo del título.
- Una referencia ausente, alterada, desconocida o no disponible no disparó FIFO, stock negativo ni una unidad inventada.
- La venta inicial y la entrega se distinguieron del cierre económico.
- Toda acción en Wallapop o Vinted siguió siendo humana.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

- "venta detectada": operación comunicada por Wallapop o Vinted que SANIA ha reconocido y todavía debe recorrer hasta su cierre
- "unidad exacta": registro lógico identificado por la referencia incluida en el anuncio vendido. Mientras no haya etiquetas físicas individuales puede satisfacerse con cualquier ejemplar idéntico disponible; cuando las unidades estén etiquetadas, la referencia queda unida a una unidad física concreta
- "pendiente de conciliación": situación en la que SANIA no puede relacionar un hecho con una operación concreta; no inventa ni altera datos y lo mantiene como revisión pendiente hasta aclararlo
- "FIFO": criterio anterior para elegir la unidad compatible más antigua; SANIA no lo usa para sustituir automáticamente una referencia física etiquetada ni para decidir qué venta gana un conflicto
- "entregada": estado logístico que no implica por sí solo cierre económico
- "cerrada": Vinted: correo final TX-COMPLETE; Wallapop: hecho económico del monedero cuya entrada en SANIA sigue bloqueada

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### El correo inicial de Vinted reservó la unidad exacta [con la app · origen: usuario]

El correo real Has vendido un artículo en Vinted aporta comprador abreviado, título, precio y, en el ejemplo, un plazo de envío de cinco días; no es la evidencia final de cierre.

- [tercero externo] Vinted envió el correo inicial Has vendido un artículo en Vinted con los campos disponibles de la operación.
- [automático: código] SANIA conservó el correo original, extrajo solo los datos realmente presentes sin completar campos ausentes y buscó la referencia de tres caracteres al final del título; un correo o campo no reconocido quedó sin aplicar hasta revisión.
- ⚠ Excepción: ¿El mismo hecho externo ya se había aplicado?
    - si sí:
        - [automático: código] SANIA conservó la operación existente sin crear otra venta, reserva ni movimiento.
        - aquí termina este camino
    - camino normal: no
- ⚑ Regla: ¿La referencia del título identificó una unidad exacta disponible?
    - si sí:
        - [automático: código] SANIA reservó de forma indivisible la unidad exacta, la excluyó del stock disponible y registró el precio del correo como importe de venta, no como beneficio definitivo.
        - …y vuelve al flujo
    - si no: referencia ausente, alterada, desconocida o unidad no disponible:
        - [automático: código] SANIA dejó la venta pendiente de conciliación, avisó a Víctor y no eligió otra unidad por FIFO ni modificó el stock.
        - aquí termina este camino
- [automático: código] Si esa misma unidad constaba publicada también en Wallapop, SANIA creó inmediatamente una tarea para que Víctor retirara manualmente el anuncio alternativo.
- [automático: código] Telegram informó a Víctor de plataforma, producto, referencia, precio y plazo disponible. Las instrucciones, el transportista, el QR o la etiqueta se obtuvieron manualmente y solo se mostraron si estaban realmente disponibles; no se inventó un recordatorio genérico de preparación.

### El correo inicial de Wallapop reservó la unidad exacta [con la app · origen: usuario]

El correo real ¡Venta confirmada! Sigue las instrucciones para enviar tu paquete aporta comprador abreviado, título, precio, total, fecha de compra y enlace a instrucciones; inicia la reserva, pero no cierra económicamente la venta.

- [tercero externo] Wallapop envió el correo ¡Venta confirmada! Sigue las instrucciones para enviar tu paquete.
- [automático: código] SANIA conservó el correo original, extrajo solo los campos realmente presentes sin completar datos ausentes y buscó la referencia de tres caracteres al final del título; un correo o campo no reconocido quedó sin aplicar hasta revisión y no se supuso que b, i y r fueran estables ni conocidos.
- ⚠ Excepción: ¿El mismo hecho externo ya se había aplicado?
    - si sí:
        - [automático: código] SANIA conservó la operación existente sin crear otra venta, reserva ni movimiento.
        - aquí termina este camino
    - camino normal: no
- ⚑ Regla: ¿La referencia del título identificó una unidad exacta disponible?
    - si sí:
        - [automático: código] SANIA reservó de forma indivisible la unidad exacta, la excluyó del stock disponible y registró el precio comunicado como importe de venta, no como beneficio definitivo.
        - …y vuelve al flujo
    - si no: referencia ausente, alterada, desconocida o unidad no disponible:
        - [automático: código] SANIA dejó la venta pendiente de conciliación, avisó a Víctor y no eligió otra unidad por FIFO ni modificó el stock.
        - aquí termina este camino
- [automático: código] Si esa misma unidad constaba publicada también en Vinted, SANIA creó inmediatamente una tarea para que Víctor retirara manualmente el anuncio alternativo.
- [automático: código] Telegram informó a Víctor de plataforma, producto, referencia, precio y datos de envío disponibles. El enlace a instrucciones fue un apoyo opcional para la acción humana, no la clave de identidad de la unidad.

### SANIA distinguió la entrega del cierre económico [con la app · origen: usuario]

- ⚑ Regla: ¿Qué hecho posterior recibió SANIA?
    - si Vinted envió La transacción se ha completado / TX-COMPLETE:
        - [automático: código] SANIA extrajo número de transacción, fecha, título, precio del artículo, precio del envío y transferencia al saldo; aplicó el hecho una sola vez por número de transacción y marcó la venta de Vinted como cerrada.
        - …y vuelve al flujo
    - si Wallapop comunicó que el paquete fue entregado:
        - [automático: código] SANIA registró la entrega y mantuvo la venta abierta porque el dinero todavía dependía del OK del comprador. No existe una rama operativa para el hecho económico final: aunque el movimiento Venta aparece en el monedero, SANIA no dispone de una fuente permitida ni de una confirmación manual acordada para observarlo.
        - …y vuelve al flujo

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- En una venta ordinaria, el correo inició la reserva; Víctor consultó manualmente conversación o instrucciones, obtuvo transportista y QR, localizó y preparó la unidad y esperó el hecho final de la plataforma. [Migración: identificador histórico E-LIVE-001; referencias históricas: D-LIVE-001, D-LIVE-003, T01-Q01, T02-Q01, T05-Q01, T05-Q04]
- Un extra o lote acordado se documentó modificando el anuncio antes de aceptar la oferta; falta un caso real que defina cómo relacionar varias unidades o productos con una sola venta. [Migración: identificador histórico E-LIVE-002; referencias históricas: D-LIVE-002, T01-Q06, T01-Q07] [G-73]
- Cuando una última unidad estaba anunciada en Wallapop y Vinted, el primer correo de venta la reservó y SANIA avisó para retirar manualmente el anuncio de la otra plataforma; dos correos simultáneos siguen sin regla de conciliación. [Migración: identificador histórico E-LIVE-003; referencias históricas: D-LIVE-001, D-LIVE-008, D-LIVE-009, T01-Q08, T04-Q04, T10-Q02] [G-71]
- Tres correos reales de Vinted con TX-COMPLETE incluyeron número de transacción, fecha, título, precio de artículo, envío y transferencia al saldo; dos ventas pudieron compartir el mismo título base con importes distintos. [Migración: identificador histórico E-LIVE-007; referencias históricas: D-LIVE-004, T06-Q01, X-LIVE-010] [G-74]
- El correo Wallapop Envíos: tu paquete ha sido entregado :) acreditó la entrega de una venta de 50,00 €, pero indicó que el dinero solo estaría disponible tras el OK del comprador. [Migración: identificador histórico E-LIVE-008; referencias históricas: D-LIVE-005, T06-Q02] [G-75]
- El historial del monedero mostró una entrada Venta de 50,00 € y también retiradas, recargas y reembolsos; asociar el hecho final exige distinguir el tipo de movimiento y una fuente permitida todavía no definida. [Migración: identificador histórico E-LIVE-009; referencias históricas: D-LIVE-005, T06-Q02, T09-Q02, X-LIVE-005]

## 5. Reglas de negocio

### G-67: Cada hecho externo se aplicó una sola vez

La idempotencia evitó duplicar venta, reserva o movimiento. Es una protección confirmada por el contexto, aunque T02-Q05 no dispone todavía de un caso real de correo duplicado, tardío o desordenado. [Migración: identificador histórico G-LIVE-001]

### G-68: Cada anuncio representó una unidad

D-LIVE-007 hizo que el anuncio identificara una sola unidad física; los extras o lotes de E-LIVE-002 requieren una relación adicional todavía no definida. [Migración: identificador histórico G-LIVE-005]

### G-69: La referencia pública vinculó correo y unidad

Según D-LIVE-006 y X-LIVE-001, el título terminó con una referencia alfanumérica de tres caracteres. El título base repetido no fue suficiente por sí solo, como muestra X-LIVE-010. [Migración: identificador histórico G-LIVE-006]

### G-70: Se reservó la unidad exacta, sin FIFO confirmado

La nueva referencia permitió reservar la unidad del anuncio. T01-Q11 no aporta un caso real de FIFO y por ello una unidad exacta no disponible abrió conciliación en vez de activar una sustitución automática. [Migración: identificador histórico G-LIVE-007]

### G-71: El primer correo reconocido reservó una unidad compartida

D-LIVE-001 y D-LIVE-009 dispusieron la reserva desde el primer correo y el aviso de retirada manual del otro anuncio; T10-Q02 mantiene abierta la colisión de correos casi simultáneos. [Migración: identificador histórico G-LIVE-008]

### G-72: Las acciones de plataforma fueron humanas

D-LIVE-010 y X-LIVE-003 impiden que SANIA cree, modifique, retire o reactive anuncios; tampoco se autorizó lectura web por X-LIVE-007. [Migración: identificador histórico G-LIVE-009]

### G-73: La descripción final determinó lo enviado

D-LIVE-002 exige incorporar manualmente al anuncio cualquier extra acordado antes de cerrar la oferta. [Migración: identificador histórico G-LIVE-010]

### G-74: Vinted cerró con TX-COMPLETE

D-LIVE-004 permite cerrar idempotentemente por número de transacción cuando llega el correo La transacción se ha completado; las variantes de idioma o plantilla siguen pendientes. [Migración: identificador histórico G-LIVE-011]

### G-75: La entrega de Wallapop no cerró la venta

D-LIVE-005 mantiene abierta la venta tras el correo de entrega. X-LIVE-005 deja bloqueado cómo incorporar a SANIA el movimiento final del monedero. [Migración: identificador histórico G-LIVE-012]

### G-76: La URL no fue una clave obligatoria

La referencia del título identificó la unidad. Por X-LIVE-002, una URL se conservó solo si estaba disponible para navegación o instrucciones, sin depender de ella para reservar. [Migración: identificador histórico D-LIVE-015]

### G-77: El importe se registró sin cerrar el beneficio

El precio comunicado se guardó como importe de venta; no se calculó aquí un beneficio definitivo porque faltan costes, comisiones, impuestos y reglas de reparto. [Migración: identificador histórico D-LIVE-030]

## 6. Estados

### venta

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| detectada | reservar la unidad exacta identificada y disponible → pasa a 'reservada, abierta' · dejar sin aplicar al stock si la identidad o disponibilidad no es segura → pasa a 'pendiente de conciliación' |
| reservada, abierta | continuar con preparación, envío y seguimiento humanos · registrar entrega logística → pasa a 'entregada, abierta' · recibir TX-COMPLETE de Vinted → pasa a 'cerrada' |
| entregada, abierta | recibir TX-COMPLETE de Vinted → pasa a 'cerrada' · mantener Wallapop abierta hasta definir cómo registrar el movimiento final del monedero |
| pendiente de conciliación | resolver identidad o disponibilidad sin aplicar FIFO automáticamente ni inventar stock |
| cerrada | conservar historial y evidencia final |

### unidad física

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| disponible | reservar por el primer correo reconocido de su anuncio → pasa a 'reservada' |
| reservada | quedar fuera del stock disponible · esperar el flujo de envío y cierre |

### anuncio alternativo de la misma unidad

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| publicado declarado | crear aviso tras la reserva → pasa a 'pendiente de retirada manual' |

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| venta | plataforma, identificadores externos realmente presentes, identificador y copia del correo de origen, título exacto comunicado y referencia de tres caracteres extraída, producto y variante reconocidos, unidad física reservada, precio del artículo o venta comunicado, precio de envío y transferencia solo cuando se informen, estado logístico separado del cierre económico, fechas de hechos y transiciones, nombre abreviado del comprador solo en la medida necesaria, con finalidad y retención pendientes, URL de instrucciones opcional, no usada como identidad, origen del momento inicial de tiempo hasta venta solo cuando se decida | correos transaccionales de Wallapop y Vinted y confirmaciones humanas permitidas |
| evidencia de cierre | plataforma, tipo de hecho, número de transacción Vinted, fecha, título con referencia cuando esté presente, importes comunicados, copia del correo o fuente permitida | TX-COMPLETE de Vinted; fuente de monedero Wallapop todavía no conectada |
| incidencia de conciliación | venta afectada, referencia ausente, alterada, desconocida o no disponible, datos observados, fecha de apertura, estado y resolución cuando se definan | SANIA cuando no pudo reservar la unidad exacta con seguridad |

- Habla con **Gmail**: recibir y conservar en solo lectura los correos transaccionales que disparan y actualizan la venta
- Habla con **Telegram**: informar de la reserva, mostrar datos disponibles, crear avisos de retirada y exponer conciliaciones a Víctor
- Habla con **Wallapop y Vinted**: usar sus correos como fuentes externas; Víctor realiza manualmente cualquier acción dentro de las cuentas

## 8. Superficie de uso

### La venta detectada en Telegram

| Campo | Valor |
|---|---|
| Quién entra | Víctor |
| Por dónde llega | mediante Telegram; el dispositivo real no quedó documentado en T10-Q06 |
| Cuándo lo usa | al aplicar el primer correo reconocido o abrir una conciliación |
| Qué ve nada más entrar | plataforma, producto, referencia exacta, precio y plazo o instrucciones solo cuando consten, además del estado abierto o bloqueado |
| Qué puede hacer | consultar qué unidad quedó reservada · abrir una instrucción o enlace opcional disponible · revisar la conciliación · actuar manualmente en la plataforma |
| Qué NO debe poder jamás | forzar FIFO para ocultar una referencia no disponible · crear una unidad o stock ficticios · confundir venta detectada o entrega con cierre económico · publicar, editar, retirar o reactivar desde SANIA |

### Avisos

| Quién se entera | De qué | Por dónde | Cuándo |
|---|---|---|---|
| Víctor | venta detectada y unidad exacta reservada | Telegram | inmediatamente después de aplicar el primer correo |
| Víctor | venta pendiente de conciliación sin cambio de stock | Telegram | cuando la referencia o disponibilidad no permitió una reserva segura |
| Víctor | anuncio alternativo de la misma unidad pendiente de retirada manual | Telegram | al reservar una unidad compartida entre plataformas; la cadencia posterior no está fijada |

### Condiciones de uso

- Una venta no reservó dos unidades aunque el correo se reprocesara.
- La referencia fue pública al final del título y no una clave privada.
- No se usaron automáticamente FIFO, b/i/r, URL, cancelación ni lectura web como reglas confirmadas.

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Responder, negociar o cerrar una conversación con el comprador.
- Publicar, editar, retirar, reasignar o reactivar automáticamente anuncios.
- Usar FIFO, una URL o los parámetros b, i y r como regla de identidad confirmada.
- Modelar una cancelación como transición operativa antes de observar un caso real.
- Resolver la prioridad de dos correos simultáneos sin una decisión adicional.
- Cerrar Wallapop a partir del correo de entrega o mediante una lectura web no autorizada.
- Calcular el beneficio definitivo o promover devoluciones y extravíos al MVP.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- T01-Q02 / X-LIVE-010 — ¿Los nuevos correos y movimientos conservan siempre el sufijo de tres caracteres y qué se hace si el título fue alterado?
- T01-Q03 — ¿Qué significan b, i y r y son estables, o se declaran fuera de uso para identificar ventas?
- T01-Q04 / T01-Q09 — ¿Qué correo y efectos reales tiene una cancelación antes o después del envío?
- T01-Q05 / T10-Q03 — ¿Qué dato mínimo del comprador se conserva, con qué finalidad, visibilidad y retención?
- T01-Q06 — ¿Cómo representa la plataforma una venta con varias unidades o productos y cómo se reparte el importe?
- T01-Q10 — ¿Qué hecho inicia la medición del tiempo hasta la venta?
- T01-Q11 — ¿Qué debe ocurrir si la unidad exacta no está disponible y existe otra compatible, dado que FIFO no tiene caso real confirmado?
- T02-Q05 — ¿Cómo se resuelve el primer correo duplicado, tardío o fuera de orden manteniendo idempotencia?
- T02-Q06 — ¿Qué patrones y campos son obligatorios en cada variante real de correo?
- T02-Q07 — ¿Cuándo puede una IA proponer datos extraídos y qué revisión humana exige antes de aplicar una reserva?
- T02-Q08 — ¿Cómo se clasifican y revisan correos desconocidos, dudosos o contradictorios sin aplicar hechos al stock?
- T04-Q02 / T04-Q05 / X-LIVE-004 — ¿Cómo se sostiene la identidad física de unidades idénticas sin etiquetas?
- T04-Q04 / T10-Q02 — ¿Qué regla concilia dos correos casi simultáneos sobre la última unidad sin crear stock negativo?
- T05-Q06 — ¿Qué prioridad tiene una confirmación humana seguida de un correo contradictorio?
- T06-Q01 — ¿Qué variantes de idioma o plantilla puede tener TX-COMPLETE?
- T06-Q02 / X-LIVE-005 — ¿Cómo recibe SANIA el cierre de Wallapop: confirmación manual o una fuente de lectura expresamente autorizada?
- T06-Q05 / T06-Q06 / T06-Q10 — ¿Qué costes y reglas convierten el importe registrado en beneficio definitivo?
- T08-Q01 — ¿Qué estados y prioridades tiene una incidencia de conciliación?
- T09-Q05 / X-LIVE-002 — ¿Se conserva una URL opcional para navegación o auditoría y qué ocurre si falla?
- T11-Q01 / T11-Q02 / T11-Q03 / X-LIVE-006 — ¿Qué primer caso real permitirá modelar devolución, extravío, estado físico y resultado económico sin promover la hipótesis anterior al MVP?

