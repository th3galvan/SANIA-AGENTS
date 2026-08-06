# Spec: Atender alertas y confirmaciones

Proyecto `sania-atender-alertas-y-confirmaciones`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

Telegram será el canal para avisos y decisiones concretas de Víctor; el dispositivo real sigue pendiente en T10-Q06. Solo se aplicarán las botoneras cuya semántica quedó confirmada; el texto libre continúa permitido para aportar contexto o abrir tickets. No se impondrán una segunda confirmación general, cadencias ni horas que la entrevista dejó pendientes.

Cuando SANIA necesitó una acción de Víctor, él recibió una tarea ligada a su operación y pudo responder con los botones confirmados o añadir contexto por texto sin que una interpretación ambigua alterara stock o dinero.

Criterios de éxito:
- Cada botón se aplicó a la tarea concreta y, cuando implicó publicar o confirmar un anuncio, a una unidad y una plataforma resueltas sin volver a pedir su identidad.
- Las tareas de publicación mostraron Enviar imágenes, Anuncio creado, Recordar más tarde y Cancelar sugerencia.
- La comprobación física ofreció Todo correcto o No OK/Abrir disputa; solo Todo correcto permitió entrada en stock.
- El texto libre siguió disponible y no modificó directamente datos sensibles por interpretación automática.
- Ningún recordatorio usó una cadencia o una hora no acordadas.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

- "botón ligado a tarea": acción que identifica la tarea concreta y, cuando corresponde a un anuncio, la unidad y plataforma resueltas; Víctor no debe volver a introducir esa identidad, aunque la granularidad de generación de tareas siga pendiente
- "No volver a recordar": botón que silencia los recordatorios de recepción; sigue pendiente decidir si además pausa, mantiene visible o cierra la tarea y nunca equivale a Todo correcto
- "texto libre": entrada permitida para contexto, dudas o tickets; la entrevista no derogó su uso global

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### Víctor respondió una tarea de publicación ligada a una plataforma [con la app · origen: usuario]

- [automático: código] SANIA presentó una acción independiente para una plataforma dentro del par secuencial del objetivo producto/unidad, con texto listo para copiar y pegar y sin adjuntar imágenes por defecto. Antes de admitir una confirmación, la acción quedó resuelta a una unidad y plataforma concretas; la agrupación de origen sigue pendiente.
- ⚑ Regla: ¿Qué botón pulsó Víctor?
    - si Enviar imágenes:
        - [automático: código] SANIA envió bajo demanda las imágenes correspondientes a esa tarea sin cambiar su estado de publicación.
        - …y vuelve al flujo
    - si Anuncio creado:
        - [automático: código] SANIA guardó que la unidad estaba publicada en la plataforma ya identificada por el mensaje.
        - …y vuelve al flujo
    - si Recordar más tarde:
        - [automático: código] SANIA registró el aplazamiento y mantuvo la tarea sin inventar cuándo debía repetir el aviso.
        - …y vuelve al flujo
    - si Cancelar sugerencia:
        - [automático: código] SANIA descartó la tarea de publicación y registró la cancelación de la sugerencia; no cambió stock ni estado de publicación porque no se acordó ningún efecto adicional.
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

Cada acción confirmable conserva su propia unidad, plataforma y estado; completar una plataforma no completa la otra. D-LIVE-021 confirma presentar el par de un objetivo antes del siguiente, mientras X-LIVE-011 deja abierta la granularidad producto/unidad de la generación. [Migración: identificador histórico G-LIVE-013; referencias históricas: D-LIVE-016]

### G-2: Las imágenes se envían bajo demanda

El mensaje inicial contiene el texto necesario; Enviar imágenes adjunta los archivos solo cuando Víctor los solicita. [Migración: identificador histórico G-LIVE-014; referencias históricas: D-LIVE-012]

### G-3: Anuncio creado persiste la publicación

El botón actualiza la unidad y plataforma ligadas a la tarea, sin pedir de nuevo su identidad. [Migración: identificador histórico G-LIVE-015; referencias históricas: D-LIVE-014]

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

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| tarea o decisión de Telegram | tipo y operación relacionada, unidad y plataforma cuando correspondan, mensaje y opciones mostradas, respuesta y actor, fecha, estado de la tarea, recordatorios y silenciamiento solicitado con el estado posterior de la tarea todavía pendiente, texto libre relacionado y ticket cuando proceda | una operación que requirió información, comprobación o acción de Víctor |

- Habla con **Telegram**: mostrar tareas, botones, imágenes bajo demanda, recordatorios y texto libre

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
- [T07-Q04] ¿Qué efecto exacto tienen Recordar más tarde, Cancelar sugerencia y No volver a recordar sobre la tarea y los datos?
- [T07-Q04, T05-Q06] ¿Cómo se resuelven dos respuestas duplicadas o contradictorias sobre la misma tarea?
- [T07-Q05, T07-Q06, T03-Q03] ¿Qué cadencia, hora y escalado necesita cada recordatorio?
- [X-LIVE-008] ¿Qué acciones adicionales debe admitir el texto libre además de aportar contexto o abrir un ticket?

