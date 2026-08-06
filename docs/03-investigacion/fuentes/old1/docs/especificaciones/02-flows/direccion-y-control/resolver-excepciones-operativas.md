# Spec: Resolver excepciones operativas

Proyecto `sania-resolver-excepciones-operativas`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

## 1. Propósito

Un hecho desconocido, ambiguo, contradictorio o imposible de relacionar quedará sin aplicar. T02-Q08 no decidió si además abre un ticket o permanece solo pendiente de revisión. Los bloqueos concretos confirmados, como la recepción No OK y el cierre de Wallapop sin fuente final, se mantienen sin inventar una regla general.

Cuando SANIA no pudo continuar con seguridad, Víctor necesitó conservar la evidencia y el trabajo correcto, ver qué operación estaba bloqueada y resolverla sin que el sistema inventara datos o reglas.

Criterios de éxito:
- Un correo desconocido o contradictorio no cambió stock, envío, cierre ni dinero.
- Cuando un caso confirmado produjo una incidencia o ticket, conservó evidencia, operación afectada e historial sin borrar los hechos anteriores.
- Solo se aplicaron continuaciones o bloqueos concretamente confirmados; la matriz general de T08-Q04 siguió pendiente.
- Wallapop permaneció bloqueado para cierre hasta decidir una fuente final permitida.

## 2. Actores y vocabulario

- **Víctor**: único responsable inicial; revisó la evidencia, aportó contexto y resolvió manualmente excepciones cuando existió una regla confirmada

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### Un hecho dudoso quedó sin aplicar [con la app]

- [automático: código] SANIA detectó un correo desconocido, un identificador insuficiente, una relación ambigua, una contradicción o un dato económico no demostrable.
- [automático: código] SANIA conservó el origen y dejó el hecho dudoso sin aplicar ni inventar el dato ausente. Su disposición como ticket o como pendiente de revisión siguió abierta en T02-Q08.
- [automático: código] SANIA aplicó únicamente los bloqueos y continuaciones ya confirmados para el caso concreto; no supuso que todo lo demás pudiera continuar porque T08-Q04 aún requiere una matriz completa.
- [persona] Cuando existió un mecanismo de revisión confirmado, Víctor pudo aportar contexto; si no existía una regla aplicable, el hecho permaneció sin aplicar. · Víctor
- [automático: código] Cuando una resolución válida pudo aplicarse, SANIA guardó actor, fecha, antes, después y motivo sin borrar el evento original.

### Wallapop quedó bloqueado después de la entrega [con la app]

- [tercero externo] Wallapop envió el correo de paquete entregado, que todavía deja el dinero pendiente del OK del comprador.
- [automático: código] SANIA registró la entrega, mantuvo abierta la venta y mostró el cierre como bloqueo conocido; no dio por decidido que ese bloqueo deba representarse como ticket.
- [automático: código] El bloqueo explicó que el hecho final aparece como movimiento de venta en el monedero, pero no existe correo final ni se ha decidido una fuente manual o automática permitida.
- [automático: código] SANIA no cerró la venta, no creó el movimiento definitivo y no calculó beneficio final.

### Una recepción no correcta quedó en incidencia [con la app]

- [persona] Después de la comprobación física, Víctor indicó No OK/Abrir disputa porque producto o cantidades no coincidían. · Víctor
- [automático: código] SANIA no creó unidades de stock y abrió una incidencia vinculada al pedido y paquete.
- [automático: código] La disputa y sus posibles reembolsos, sustituciones o devoluciones quedaron pendientes de definición y no se resolvieron por hipótesis.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- El correo real de Wallapop de paquete entregado no fue un cierre; el movimiento final solo se observó en el historial del monedero, fuente que SANIA aún no puede recibir. [E-LIVE-008, E-LIVE-009, T06-Q02, X-LIVE-005]
- Los productos defectuosos, faltantes o equivocados no cuentan con un caso real; Víctor indicó hipotéticamente que abriría disputa, por lo que no se debe inventar el flujo de resolución. [T03-Q01, T11-Q03]

## 5. Reglas de negocio

### G-LIVE-001: Un hecho externo se aplica una sola vez

Un duplicado reconocido no repite sus efectos; si no puede determinarse que sea el mismo hecho, queda sin aplicar y T02-Q08 decidirá si además se abre un ticket.

### G-LIVE-004: Una recepción no correcta no crea stock

No OK/Abrir disputa bloquea la entrada y crea una incidencia sin inventar unidades.

### G-LIVE-012: La entrega de Wallapop no resuelve el cierre

La venta permanece abierta y el bloqueo continúa hasta que exista una fuente final acordada; su representación como ticket no está decidida.

### X-LIVE-007: No se autoriza lectura web por hipótesis

La mención a Playwright contradijo la limitación posterior a correo; en el MVP no se usa para resolver automáticamente tickets ni el cierre de Wallapop.

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| ticket operativo | tipo y estado sin inventar un catálogo no acordado, operación y transición afectadas, origen y evidencia, descripción del dato desconocido, ambiguo o contradictorio, bloqueo concreto confirmado y efectos conocidos; continuaciones generales pendientes de T08-Q04, responsable inicial Víctor, historial, contexto y resolución, antes, después, actor, fecha y motivo cuando se aplique una corrección, recordatorios solo cuando exista una cadencia definida | casos para los que un flujo confirmado decide crear una incidencia o ticket; no todo hecho dudoso lo crea automáticamente |

- Habla con **Telegram**: avisar a Víctor y recoger contexto estructurado o texto libre sin aplicar automáticamente una interpretación ambigua
- Habla con **Gmail**: conservar el correo que originó la excepción

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Ocultar una excepción inventando un dato, una transición o una fuente.
- Aplicar una regla general de detener o continuar actividades sin la matriz pendiente de T08-Q04.
- Borrar el origen o el historial después de resolver.
- Cerrar Wallapop mediante correo de entrega, precio inicial o un OK manual todavía no acordado.
- Usar Playwright o lectura web para resolver el bloqueo sin una autorización explícita.
- Asignar prioridades, estados, plazos o cadencias de ticket que aún no fueron definidos.
- Implementar devoluciones, extravíos o resoluciones de disputa a partir de respuestas hipotéticas.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T02-Q08] ¿Qué regla general distinguirá correo desconocido, lectura dudosa y contradicción, y cuándo basta con dejarlo sin aplicar?
- [T05-Q06] ¿Qué prioridad tendrá una confirmación manual si después llega un correo contradictorio?
- [T06-Q02, X-LIVE-005] ¿Qué fuente permitirá resolver el bloqueo de cierre de Wallapop?
- [T08-Q01] ¿Qué estados y prioridades tendrá un ticket?
- [T08-Q02] ¿Cuándo podrá cambiar el responsable inicial, que hoy es Víctor?
- [T08-Q03, T07-Q05] ¿Qué plazos, recordatorios y escalados tendrá cada clase de ticket?
- [T08-Q04] ¿Qué excepciones bloquean una transición y cuáles permiten continuar el resto?
- [T10-Q02] ¿Cómo se concilian dos hechos casi simultáneos o contradictorios sobre la misma operación?
- [T11-Q01, T11-Q02, T11-Q03] ¿Qué primeros casos reales permitirán modelar devoluciones y extravíos fuera del MVP?

