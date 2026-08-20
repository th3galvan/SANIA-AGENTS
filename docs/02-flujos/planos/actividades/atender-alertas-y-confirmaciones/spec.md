# Spec: Atender alertas y confirmaciones

Proyecto `sania-atender-alertas-y-confirmaciones`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

Telegram será el canal para avisos y decisiones concretas de Víctor; el dispositivo real sigue pendiente en T10-Q06. Solo se aplicarán las botoneras cuya semántica quedó confirmada; el texto libre continúa permitido para aportar contexto o abrir tickets. Recordar más tarde vuelve a presentar las tareas de publicación a las 18:00 del día siguiente según la hora local de SANIA; no se impondrán una segunda confirmación general ni otras cadencias que la entrevista dejó pendientes.

Cuando SANIA necesitó una acción de Víctor, él recibió una tarea ligada a su operación y pudo responder con los botones confirmados o añadir contexto por texto sin que una interpretación ambigua alterara stock o dinero.

Criterios de éxito:
- Cada botón se aplicó a la tarea concreta y, cuando implicó confirmar un anuncio creado personalmente por Víctor, SANIA solicitó el enlace antes de marcar como publicados el producto, la plataforma y la referencia ya asignados.
- Las tareas de publicación mostraron Enviar imágenes, Anuncio creado, Recordar más tarde y Cancelar sugerencia; los anuncios confirmados permitieron Corregir enlace y, cuando SANIA cuestionó o recibió repetido un enlace, las confirmaciones correspondientes también desde Telegram.
- La comprobación física ofreció Todo correcto o No OK/Abrir disputa; solo Todo correcto permitió entrada en stock.
- El texto libre siguió disponible y no modificó directamente datos sensibles por interpretación automática.
- Las tareas de publicación aplazadas con Recordar más tarde reaparecieron a las 18:00 del día siguiente según la hora local de SANIA; ningún otro recordatorio usó una cadencia o una hora no acordadas.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

- "botón ligado a tarea": acción que identifica la tarea concreta y, cuando corresponde a un anuncio, el producto, la plataforma y la referencia asignada al crearlo; Víctor no debe volver a introducir esa identidad
- "No volver a recordar": botón que silencia los recordatorios de recepción; sigue pendiente decidir si además pausa, mantiene visible o cierra la tarea y nunca equivale a Todo correcto
- "texto libre": entrada permitida para contexto, dudas o tickets; la entrevista no derogó su uso global
- "El enlace es correcto": acción de Telegram que acepta definitivamente un enlace cuestionado para la tarea o el anuncio ligado y evita que SANIA vuelva a preguntarlo

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### Víctor respondió una tarea de publicación ligada a una plataforma [con la app · origen: usuario]

- [automático: código] SANIA presentó una tarea independiente para la plataforma dentro del par del producto, con texto listo para copiar y pegar y sin adjuntar imágenes por defecto. Antes de admitir una confirmación, la tarea quedó resuelta al producto, la plataforma y la referencia disponible que SANIA asignó al crear el anuncio.
- ⚑ Regla: ¿Qué botón pulsó Víctor?
    - si Enviar imágenes:
        - [automático: código] SANIA envió bajo demanda las imágenes asociadas al producto de esa tarea sin cambiar su estado de publicación.
        - …y vuelve al flujo
    - si Anuncio creado:
        - [automático: código] SANIA solicitó el enlace del anuncio y mantuvo la tarea pendiente sin cambiar el estado de publicación.
        - [persona] Víctor envió el enlace del anuncio que había creado personalmente. · Víctor
        - [automático: código] Si SANIA no cuestionó el enlace, lo guardó y marcó como publicados el producto y la referencia asignada en la plataforma ya identificada por la tarea. Si lo cuestionó, ofreció enviar por mensaje el enlace correcto o pulsar El enlace es correcto; cualquiera de las dos acciones lo aceptó y evitó nuevas preguntas para ese enlace.
        - …y vuelve al flujo
    - si Recordar más tarde:
        - [automático: código] SANIA registró el aplazamiento, mantuvo la tarea pendiente sin cambiar el stock ni el estado del anuncio y volvió a presentarla a las 18:00 del día siguiente según su hora local.
        - …y vuelve al flujo
    - si Cancelar sugerencia:
        - [automático: código] SANIA descartó la tarea, registró la cancelación y dejó de generar sugerencias automáticas para ese producto en esa plataforma hasta que Víctor pulsara Volver a sugerir. No cambió el stock ni marcó el producto como publicado; mientras siguieran desactivadas, un anuncio posterior debía vincularse manualmente.
        - …y vuelve al flujo
- [automático: código] SANIA conservó la respuesta, la tarea, la plataforma, la unidad, el actor y la fecha fuera de Telegram.

### Víctor respondió después de comprobar físicamente el paquete recibido [con la app · origen: usuario]

- [automático: código] Después de que el tracking indicara entrega, SANIA pidió comprobar físicamente producto y cantidades; el tracking por sí solo no sumó stock.
- ⚑ Regla: ¿Qué respuesta estructurada dio Víctor después de la comprobación?
    - si Todo correcto:
        - [automático: código] SANIA registró las unidades en stock y generó inmediatamente las tareas de publicación correspondientes.
        - …y vuelve al flujo
    - si No OK / Abrir disputa:
        - [automático: código] SANIA no creó stock y abrió una incidencia; el flujo interno completo de disputa permanece pendiente.
        - …y vuelve al flujo

### Víctor corrigió desde Telegram el enlace de un anuncio [con la app · origen: usuario]

- [automático: código] Telegram mostró la acción Corregir enlace en un contexto ligado a un anuncio concreto.
- [persona] Víctor pulsó Corregir enlace y envió el enlace correcto. · Víctor
- [automático: código] Si SANIA no cuestionó el enlace nuevo, actualizó el enlace del mismo anuncio, eliminó completamente el anterior sin conservarlo en el historial y no cambió el producto, la plataforma, la referencia ni el stock. Si lo cuestionó, Víctor pudo enviar por mensaje el enlace correcto o pulsar El enlace es correcto; cualquiera de las dos acciones lo aceptó y evitó nuevas preguntas para ese enlace.

### Víctor confirmó desde Telegram un enlace repetido [con la app · origen: usuario]

- [automático: código] Telegram avisó a Víctor de que SANIA ya había recibido exactamente el mismo enlace para la misma tarea y le preguntó si estaba seguro.
- ⚑ Regla: ¿Víctor confirmó que estaba seguro?
    - si no:
        - [automático: código] SANIA conservó el estado que ya tenía y no trató el enlace repetido como una confirmación.
        - aquí termina este camino
    - camino normal: sí, SANIA aceptó el enlace como El enlace es correcto y no volvió a preguntarlo

### SANIA recordó una comprobación sin cerrar la tarea por silencio [con la app · origen: usuario]

- [automático: código] Si Víctor no respondió, SANIA mantuvo pendiente la comprobación y pudo recordarla sin asumir que el paquete estaba correcto.
- [persona] Víctor pudo pulsar No volver a recordar. · Víctor
- [automático: código] SANIA registró que Víctor pulsó No volver a recordar y silenció los avisos futuros de esa comprobación, pero no interpretó por su cuenta si la tarea debía pausarse, cerrarse o mantenerse visible; ese efecto sobre el estado sigue por definir.

### Víctor añadió contexto por texto libre [con la app · origen: usuario]

- [persona] Víctor envió un texto libre con contexto, una duda, una corrección propuesta o la descripción de una excepción. · Víctor
- [automático: código] SANIA relacionó el texto cuando pudo o abrió un ticket, pero no cambió directamente stock, estados críticos ni dinero a partir de una interpretación ambigua.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- Para cada publicación, Víctor confirmó una botonera con Enviar imágenes, Anuncio creado, Recordar más tarde y Cancelar sugerencia; las tareas de Wallapop y Vinted son mensajes independientes. [Migración: referencias históricas: D-LIVE-013, D-LIVE-016, T07-Q01]
- Ante la entrega de AliExpress, Telegram debe pedir Todo correcto o No OK/Abrir disputa; solo la comprobación física correcta suma stock. [Migración: referencias históricas: D-LIVE-017, D-LIVE-018, D-LIVE-020, T03-Q03, E-LIVE-004] [G-7]
- La entrevista confirmó botones concretos, pero no confirmó una prohibición global de texto libre. [Migración: referencias históricas: T07-Q04] [G-8]

## 5. Reglas de negocio

### G-1: Las tareas de publicación son independientes por plataforma

Cada producto genera una tarea para Wallapop y otra para Vinted. Si hay al menos dos unidades disponibles, las tareas reciben referencias distintas; si solo hay una, comparten esa referencia. Cada una conserva su plataforma y su propio estado. [Migración: identificador histórico G-LIVE-013; referencias históricas: D-LIVE-016; decisión cerrada de X-LIVE-011]

### G-2: Las imágenes se envían bajo demanda

El mensaje inicial contiene el texto necesario; Enviar imágenes adjunta solo las imágenes asociadas al producto cuando Víctor las solicita. Su formato y cantidad se decidirán al definir la generación, con compatibilidad para Wallapop y Vinted. [Migración: identificador histórico G-LIVE-014; referencias históricas: D-LIVE-012]

### G-3: Anuncio creado solicita el enlace antes de persistir

Víctor crea personalmente el anuncio. El botón solicita su enlace y solo al recibirlo actualiza el producto y la referencia asignada en la plataforma ligada a la tarea; una pulsación sin enlace mantiene la tarea pendiente. [Migración: identificador histórico G-LIVE-015; referencias históricas: D-LIVE-014]

### G-4: El tracking entregado no suma stock

Todo correcto solo es válido después de que Víctor compruebe físicamente producto y cantidades. [Migración: identificador histórico G-LIVE-002; referencias históricas: D-LIVE-017]

### G-5: Todo correcto permite stock y tareas de anuncio

Después de comprobar físicamente producto y cantidades, Todo correcto registra las unidades y genera inmediatamente las tareas de publicación. [Migración: identificador histórico G-LIVE-003; referencias históricas: D-LIVE-020]

### G-6: No OK bloquea la entrada

No OK/Abrir disputa crea una incidencia y no inventa unidades. [Migración: identificador histórico G-LIVE-004; referencias históricas: D-LIVE-018]

### G-7: El silencio no confirma la recepción

SANIA mantiene pendiente la comprobación, puede recordarla y ofrece No volver a recordar; la cadencia y el efecto final del silenciamiento siguen abiertos. [Migración: identificador histórico D-LIVE-019]

### G-8: El texto libre no fue derogado

Las botoneras confirmadas conviven con el texto libre para contexto y tickets; una frase ambigua no aplica por sí sola un cambio crítico. [Migración: identificador histórico X-LIVE-008]

### G-97: Las tareas de publicación se recuerdan al día siguiente

Recordar más tarde conserva pendiente la tarea de publicación y hace que SANIA vuelva a presentarla a las 18:00 del día siguiente según su hora local, sin cambiar el stock ni el estado del anuncio.

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| tarea o decisión de Telegram | tipo y operación relacionada, producto, plataforma y referencia asignada cuando correspondan, mensaje y opciones mostradas, respuesta y actor, enlace del anuncio cuando Víctor confirmó una publicación y su aceptación definitiva cuando pulsó El enlace es correcto, fecha, estado de la tarea, recordatorios y silenciamiento solicitado con el estado posterior de la tarea todavía pendiente, texto libre relacionado y ticket cuando proceda | una operación que requirió información, comprobación o acción de Víctor |

- Habla con **Telegram**: mostrar tareas, botones, imágenes bajo demanda, recordatorios, texto libre, la acción para corregir el enlace de un anuncio, El enlace es correcto cuando SANIA lo cuestione y la confirmación de un enlace repetido

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Usar Telegram como única fuente de verdad de las operaciones.
- Prohibir globalmente el texto libre: esa interpretación no fue confirmada.
- Permitir que un texto ambiguo modifique directamente stock o dinero.
- Pedir una segunda confirmación general para todo hecho físico, stock o dinero: la lista sigue pendiente.
- Asignar una hora, frecuencia o umbral de 48 horas no acordados.
- Dar por cerrada una tarea de recepción porque Víctor no respondió o silenció los avisos.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T07-Q01] ¿Qué mecanismo técnico reconocerá a Víctor como único usuario inicial del bot?
- [T07-Q02 / D-LIVE-010] La división general está confirmada —SANIA registra y prepara; Víctor confirma hechos físicos y actúa en plataformas—: ¿qué excepciones concretas faltan para completar la matriz de autonomía?
- [T07-Q03, T06-Q08] ¿Qué acciones concretas, si alguna, requieren una segunda confirmación?
- [T07-Q04] ¿Qué efecto exacto tiene No volver a recordar sobre la tarea y los datos?
- [T07-Q04, T05-Q06] ¿Cómo se resuelven dos respuestas duplicadas o contradictorias, incluido recibir dos enlaces iguales o diferentes para la misma tarea?
- [T07-Q06, T03-Q03] ¿Qué cadencia, hora y escalado necesitan los demás recordatorios?
- [X-LIVE-008] ¿Qué acciones adicionales debe admitir el texto libre además de aportar contexto o abrir un ticket?

