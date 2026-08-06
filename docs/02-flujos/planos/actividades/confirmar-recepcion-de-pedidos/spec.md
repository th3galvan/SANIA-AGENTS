# Spec: Confirmar la recepción de pedidos

Proyecto `sania-confirmar-recepcion-de-pedidos`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

Esta actividad recoge la comprobación física que un tracking entregado no puede demostrar. La confirmación se realiza sobre el paquete y su contenido observado; solo Todo correcto permite crear las unidades correspondientes y No OK o Abrir disputa bloquea la entrada.

Cuando el seguimiento indicó que un paquete había llegado, Víctor necesitó comprobar en mano productos y cantidades y responder por Telegram antes de que SANIA creara stock.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

- "Todo correcto": confirmación humana de que Víctor tuvo el contenido en las manos y que productos y cantidades concordaron; no es una confirmación del tracking
- "No OK / Abrir disputa": respuesta que bloquea la creación de unidades y abre una incidencia; el nombre definitivo del botón y el flujo interno de disputa siguen pendientes
- "No volver a recordar": botón que silencia los recordatorios de recepción; sigue pendiente decidir si además pausa, mantiene visible o cierra la tarea y nunca equivale a Todo correcto

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### Víctor confirmó o rechazó el contenido físico de un paquete [con la app · origen: usuario]

- [automático: código] SANIA reconoció que el seguimiento comunicó la entrega de un paquete y lo dejó pendiente de comprobación física sin crear stock.
- [automático: código] SANIA mostró por Telegram únicamente los datos comprobados disponibles para ayudar a comparar el paquete, señalando cualquier relación dudosa. Los campos exactos y la forma de representar líneas o paquetes parciales siguen pendientes en T03-Q02 y T02-Q09.
- [persona] Víctor tuvo el contenido en las manos, contó e identificó los productos y los comparó con el pedido. · Víctor
- ⚑ Regla: ¿Víctor pulsó Todo correcto?
    - si no, pulsó No OK o Abrir disputa:
        - [automático: código] SANIA mantuvo bloqueada la entrada del paquete y abrió una incidencia sin crear referencias ni unidades.
        - [automático: código] La incidencia quedó abierta y el stock bloqueado. No se asumió que Víctor abriera o gestionara una disputa en AliExpress: A-018 fue hipotético y A-048 dejó sin recuperar el flujo real.
        - aquí termina este camino
    - camino normal: sí, confirmó productos y cantidades
- [automático: código] SANIA envió el contenido físicamente confirmado a Dar entrada a las unidades recibidas; allí solo Stock para venta crea unidades de inventario y lanza tareas de anuncio.
- [automático: código] Si el pedido tenía más paquetes pendientes, el pedido permaneció abierto aunque este paquete hubiera quedado comprobado.

### SANIA recordó una comprobación sin respuesta [con la app · origen: usuario]

- ⚑ Regla: ¿La tarea de comprobación seguía sin respuesta?
    - si no:
        - [automático: código] SANIA no envió otro aviso para una tarea ya resuelta.
        - aquí termina este camino
    - camino normal: sí, SANIA envió un recordatorio con Todo correcto, No OK o Abrir disputa y No volver a recordar
- [automático: código] La cadencia, el escalado y el momento del primer recordatorio quedaron sin fijar.
- ⚠ Excepción: ¿Víctor pulsó No volver a recordar?
    - si semántica operativa pendiente:
        - [automático: código] SANIA silenció nuevos recordatorios, pero no se ha decidido si la tarea queda pausada, cerrada o visible; ninguna opción puede crear stock automáticamente.
        - …y vuelve al flujo
    - camino normal: SANIA registró la elección sin interpretarla como recepción correcta

### Víctor corrigió una confirmación equivocada [con la app · origen: usuario]

- [persona] Víctor solicitó corregir una confirmación o un dato físico y explicó el motivo. · Víctor
- [automático: código] SANIA conservó valor anterior, valor nuevo, actor, fecha y hora y motivo, sin borrar el evento original.
- [automático: código] El procedimiento exacto para deshacer unidades o tareas ya creadas y resolver un correo posterior contradictorio sigue pendiente.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- Los productos de distintos vendedores pueden llegar separados o consolidados y a cuentagotas; Víctor solo considera completamente recibido lo que tiene en las manos y concuerda en producto y cantidad. [Migración: identificador histórico E-LIVE-004; referencias históricas: T02-Q03, T02-Q09, T03-Q01, D-LIVE-017]

## 5. Reglas de negocio

### G-19: Tracking entregado no crea stock

La entrega logística abre una comprobación física y no aumenta existencias. [Migración: identificador histórico G-LIVE-002; referencias históricas: D-LIVE-017]

### G-20: Todo correcto autoriza la entrada

Todo correcto confirma producto y cantidades comprobados físicamente y permite registrar las unidades y lanzar las tareas de anuncio que correspondan al stock para venta. [Migración: identificador histórico G-LIVE-003; referencias históricas: D-LIVE-017, D-LIVE-020]

### G-21: No OK bloquea la entrada

No OK o Abrir disputa abre una incidencia y no inventa ni crea unidades. [Migración: identificador histórico G-LIVE-004; estado histórico: confirmada en intención; el flujo de disputa está pendiente; referencias históricas: D-LIVE-018, T03-Q01, T03-Q09]

### G-22: Recordatorios de recepción sin cadencia fijada

SANIA recuerda una comprobación sin respuesta y ofrece No volver a recordar, que silencia nuevos avisos; la frecuencia previa y el efecto del silenciamiento sobre el estado de la tarea no están definidos. [Migración: identificador histórico D-LIVE-019; referencias históricas: T03-Q03, T07-Q04, T07-Q05]

## 6. Estados

### paquete

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| entregado pendiente de comprobación | Víctor pulsó Todo correcto tras revisar productos y cantidades (Víctor) → pasa a 'recibido correcto' · Víctor pulsó No OK o Abrir disputa (Víctor) → pasa a 'incidencia' |
| recibido correcto |  |
| incidencia |  |

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| comprobación física | pedido y paquete, contenido esperado sustentado por evidencia, productos y cantidades confirmados, respuesta de Víctor, actor, fecha y hora, evento original y correcciones | respuesta de Víctor en Telegram |
| recordatorio de recepción | tarea concreta, avisos enviados, respuesta, selección No volver a recordar sin semántica inferida | tarea pendiente y respuestas de Telegram |

- Habla con **Telegram**: mostrar la comprobación y recoger Todo correcto, No OK o Abrir disputa y No volver a recordar
- Habla con **Seguimiento de pedidos de AliExpress**: recibir el paquete entregado sin convertirlo en stock
- Habla con **Dar entrada a las unidades recibidas**: continuar únicamente después de Todo correcto

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Dar por recibido un paquete únicamente porque lo dijo el seguimiento.
- Crear unidades, referencias o tareas de anuncio antes de Todo correcto.
- Asumir que cada correo o cada línea equivale a un paquete físico.
- Permitir que No OK o Abrir disputa cree parcialmente unidades sin una regla confirmada.
- Automatizar la apertura, negociación o cierre de una disputa dentro de AliExpress.
- Fijar una cadencia o una semántica de No volver a recordar no confirmadas.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T03-Q01] ¿Qué debe ocurrir en el primer caso real de producto incompleto, defectuoso o con variante errónea?
- [T03-Q02 / T02-Q09] ¿Qué datos necesita ver Víctor para distinguir productos o líneas parecidas y cómo se muestran paquetes o pedidos parcialmente recibidos?
- [T03-Q03 / T07-Q04 / T07-Q05] ¿Cuándo se recuerda, con qué cadencia y qué efecto exacto tiene No volver a recordar?
- [T03-Q04] ¿Cómo se deshace una confirmación equivocada y sus unidades derivadas sin perder la historia?
- [T05-Q06] ¿Qué prioridad tiene una confirmación manual si después llega un correo contradictorio?
- [D-LIVE-018] ¿El botón definitivo se llamará No OK, Abrir disputa o mostrará ambas acciones?

