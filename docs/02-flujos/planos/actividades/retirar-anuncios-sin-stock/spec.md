# Spec: Retirar anuncios cuando se agota el stock

Proyecto `sania-retirar-anuncios-sin-stock`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

Cuando el primer correo de venta reserva una unidad que también consta anunciada en la otra plataforma, SANIA crea de inmediato una tarea para que Víctor retire manualmente el anuncio alternativo. La tarea se identifica por unidad, referencia, título y plataforma; una URL es opcional. No se ha fijado la evidencia de retirada, el botón de confirmación ni una cadencia diaria.

Cuando la última unidad quedó reservada desde el primer correo, Víctor necesitó saber qué anuncio alternativo de esa misma unidad debía retirar para evitar una segunda venta.

Criterios de éxito:
- La unidad quedó reservada antes de solicitar la retirada.
- La tarea señaló la unidad y la plataforma correctas sin depender de un enlace.
- Víctor realizó la retirada dentro de Wallapop o Vinted.
- SANIA mantuvo la retirada pendiente de acreditar porque no se definieron la acción ni la evidencia que permiten darla por confirmada, y no leyó la web para comprobarla.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

- "anuncio alternativo": anuncio de la otra plataforma que apunta a la misma unidad física ya reservada
- "pendiente de retirada": estado interno que indica que Víctor todavía debe retirar o confirmar manualmente el anuncio; no prueba su visibilidad real
- "retirada pendiente de acreditar": situación posterior a la acción manual en la que SANIA aún no puede cerrar la tarea porque T08-Q05 no definió mecanismo ni evidencia

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### Víctor retiró manualmente el anuncio alternativo [con la app · origen: usuario]

- [tercero externo] Llegó el primer correo reconocido de venta de Wallapop o Vinted.
- [automático: código] SANIA reservó primero la unidad exacta identificada por la referencia de tres caracteres al final del título.
- ⚑ Regla: ¿La misma unidad constaba publicada también en la otra plataforma?
    - si sí:
        - [automático: código] SANIA creó inmediatamente una tarea pendiente de retirada con la unidad, referencia, título y plataforma; añadió la URL solo si ya estaba guardada como dato opcional.
        - [persona] Víctor abrió Wallapop o Vinted y retiró manualmente el anuncio. · Víctor
        - [automático: código] SANIA mantuvo la tarea en retirada pendiente de acreditar. No esperó una declaración, captura o botón concreto como si ya estuviera aprobado: la evidencia, el mecanismo de confirmación y la cadencia de recordatorio siguen pendientes.
        - …y vuelve al flujo
    - camino normal: no, no hubo anuncio alternativo que retirar

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- Una última unidad estuvo anunciada a la vez en Wallapop y Vinted. El primer correo de venta la reservó y Víctor eliminó manualmente el otro anuncio; la colisión de dos correos simultáneos no quedó resuelta. [Migración: identificador histórico E-LIVE-003; referencias históricas: E-LIVE-003, D-LIVE-001, D-LIVE-008, D-LIVE-009, T01-Q08, T04-Q04, T10-Q02] [G-83]

## 5. Reglas de negocio

### G-82: Cada anuncio representó una unidad

D-LIVE-007 estableció una relación anuncio-unidad. Con varias unidades, Wallapop y Vinted apuntan normalmente a referencias distintas; la retirada de este flujo se dirige al anuncio alternativo de la unidad ya reservada. [Migración: identificador histórico G-LIVE-005]

### G-83: El primer correo ganó para una unidad compartida

D-LIVE-001 y D-LIVE-009 dispusieron reservar desde el primer correo reconocido y solicitar inmediatamente la retirada manual del anuncio de la otra plataforma; T10-Q02 mantiene pendiente la concurrencia casi simultánea. [Migración: identificador histórico G-LIVE-008]

### G-84: La retirada fue humana

D-LIVE-010 y X-LIVE-003 prohíben que SANIA elimine, pause, edite o reactive anuncios dentro de Wallapop o Vinted. [Migración: identificador histórico G-LIVE-009]

### G-85: La URL no fue obligatoria

La tarea se vinculó por unidad, plataforma y referencia del título. X-LIVE-002 deja la URL como dato opcional para navegación o auditoría, no como requisito. [Migración: identificador histórico D-LIVE-015]

## 6. Estados

### anuncio alternativo

| Estado | Qué se puede hacer (quién, y a qué estado pasa) |
|---|---|
| publicado declarado | reservar la unidad compartida desde el primer correo → pasa a 'pendiente de retirada manual' |
| pendiente de retirada manual | mostrar la tarea a Víctor sin ejecutar la retirada · registrar recordatorios solo cuando se defina su cadencia · mantener la retirada pendiente de acreditar hasta resolver T08-Q05 |

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| tarea de retirada | venta que reservó la unidad, unidad física y referencia de tres caracteres, plataforma del anuncio alternativo, título conocido, URL opcional si fue aportada, estado de la tarea, fecha de creación, recordatorios solo cuando se defina la cadencia, fecha, actor y evidencia de retirada solo cuando existan | primer correo reconocido y estado interno de publicación; el origen de la acreditación final sigue pendiente |

- Habla con **Telegram**: entregar la tarea identificada; la recogida de una acreditación solo se incorporará cuando se defina el mecanismo
- Habla con **Wallapop y Vinted**: ser el lugar donde Víctor realiza manualmente la retirada; SANIA no lee ni modifica la plataforma

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Eliminar, pausar, editar o reactivar automáticamente anuncios.
- Retirar anuncios de unidades distintas que continúan disponibles.
- Exigir una URL para identificar el anuncio alternativo.
- Comprobar la retirada leyendo perfiles públicos o iniciando sesión.
- Repetir el aviso una vez al día o a una hora fija sin decisión expresa.
- Restaurar stock o reactivar anuncios automáticamente ante una cancelación no documentada.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- T01-Q09 — ¿Qué ocurre con esta tarea y con la unidad si llega una cancelación real, y qué correo la prueba?
- T04-Q04 / T10-Q02 — ¿Cómo se concilian dos correos casi simultáneos sobre la última unidad?
- T07-Q05 / T07-Q06 — ¿Con qué cadencia, hora y zona horaria se recuerda una retirada pendiente?
- T08-Q05 — ¿Qué acción y evidencia confirman que Víctor retiró todos los anuncios relacionados?
- T09-Q01 / X-LIVE-007 — ¿La comprobación seguirá siendo exclusivamente humana o se autorizará en el futuro alguna lectura pública?
- T09-Q05 / X-LIVE-002 — Si se conserva una URL opcional, ¿para qué finalidad y qué se hace si cambia o falla?

