# Spec: Comprobar qué productos están publicados

Proyecto `sania-comprobar-productos-publicados`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

En el MVP, SANIA solo puede mostrar el estado interno derivado de Anuncio creado y de las tareas pendientes. No está autorizada la lectura pública de perfiles, el uso de Playwright, el inicio de sesión ni una revisión web semanal; tampoco se definió cómo Víctor comunicaría y acreditaría una comprobación manual. Por tanto, la visibilidad real en Wallapop o Vinted no es verificable por el flujo actual.

Cuando el stock o una tarea de anuncio cambió, Víctor necesitó ver qué publicaciones constaban internamente y distinguir ese dato de una visibilidad real que SANIA todavía no puede verificar.

Criterios de éxito:
- Los estados internos de Wallapop y Vinted se mantuvieron separados por unidad y plataforma.
- SANIA distinguió un estado interno declarado de una visibilidad real no verificable con las fuentes y mecanismos actuales.
- Ninguna comprobación pública, periódica o autenticada se ejecutó sin una autorización posterior explícita.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

- "publicado declarado": estado persistido al pulsar Anuncio creado para una tarea ligada a una unidad y plataforma; no prueba por sí solo que el anuncio continúe visible
- "visibilidad real no verificable": estado actual del anuncio en la plataforma que SANIA no puede acreditar con las fuentes autorizadas; T08-Q05 deja pendientes tanto la evidencia como el mecanismo de comunicación
- "lectura pública": consulta automática de un perfil o anuncio sin iniciar sesión; está bloqueada y no autorizada en el MVP

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### SANIA comparó stock y estados internos [con la app · origen: usuario]

- [automático: código] SANIA reunió por unidad los estados internos de Wallapop y Vinted procedentes de Anuncio creado y de las tareas de retirada todavía pendientes de acreditar.
- [automático: código] SANIA comparó esos estados con la disponibilidad o reserva de la unidad y señaló incoherencias internas, sin afirmar que había observado el anuncio en la web.
- ⚠ Excepción: ¿La consulta exigía conocer la visibilidad real y no solo el estado interno?
    - si sí:
        - [automático: código] SANIA marcó el resultado como no verificable y abrió o mantuvo visible el bloqueo T08-Q05. No solicitó ni registró una declaración, captura o lectura web como si su mecanismo ya estuviera decidido.
        - …y vuelve al flujo
    - camino normal: no, SANIA mostró el registro interno con su origen y fecha

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- La última unidad pudo constar como publicada a la vez en Wallapop y Vinted; tras el primer correo de venta fue necesario pedir a Víctor que retirara manualmente el otro anuncio. [Migración: identificador histórico E-LIVE-003; referencias históricas: D-LIVE-001, D-LIVE-008, D-LIVE-009, G-LIVE-008, T08-Q05]
- Víctor revisó manualmente conversaciones, fotos y descripción cuando un anuncio rendía poco; no se definieron una métrica ni una lectura pública automática para medirlo. [Migración: identificador histórico E-LIVE-006; referencias históricas: D-LIVE-029, T09-Q02, T09-Q04]

## 5. Reglas de negocio

### G-15: SANIA no actuó en las plataformas

Víctor realizó toda creación, edición, retirada, reactivación y publicación. La posible lectura pública quedó sin autorizar por T09-Q01 y X-LIVE-007, así que no forma parte del MVP. [Migración: identificador histórico G-LIVE-009]

### G-16: Los estados se mantuvieron separados por plataforma

Una unidad pudo tener estados distintos en Wallapop y Vinted; confirmar o corregir uno no alteró automáticamente el otro. [Migración: identificador histórico G-LIVE-013]

### G-17: Anuncio creado registró una declaración persistente

D-LIVE-014 permitió registrar unidad y plataforma desde la tarea concreta, pero esa confirmación no se reinterpretó como una inspección continua de la web. [Migración: identificador histórico G-LIVE-015]

### G-18: La URL no fue obligatoria

La referencia de tres caracteres del título identificó la unidad. Según X-LIVE-002, un enlace solo se usó si estaba disponible y nunca como requisito para ejecutar la comprobación interna. [Migración: identificador histórico D-LIVE-015]

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| estado declarado de anuncio | unidad y referencia de tres caracteres, plataforma, título conocido, estado interno, origen de la declaración, fecha y actor, URL opcional si fue aportada, duda o incoherencia pendiente, marca explícita de visibilidad real no verificable | tareas de publicación y retirada; no existe todavía una fuente de verificación de visibilidad real |

- Habla con **Telegram**: mostrar estados internos e incoherencias y señalar que la visibilidad real no es verificable; no recoge todavía una declaración de comprobación

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Consultar perfiles o anuncios públicos de Wallapop o Vinted en el MVP.
- Iniciar sesión, usar Playwright, scraping o cualquier navegador automatizado.
- Programar una revisión web semanal sin decisión de día, hora, alcance y permiso.
- Modificar, republicar, retirar o cambiar precios.
- Interpretar Anuncio creado como prueba permanente de visibilidad pública.
- Inventar un procedimiento de CAPTCHA o reactivación para una integración no autorizada.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- T09-Q01 / X-LIVE-007 — ¿Se autoriza en alguna evolución una lectura pública sin sesión o el alcance seguirá limitado a Gmail y a mecanismos humanos que aún deben definirse?
- T09-Q02 — Si se autorizara una lectura, ¿qué información pública mínima y qué finalidad legítima tendría?
- T09-Q03 — Si se autorizara una lectura, ¿qué procedimiento humano se seguiría ante bloqueo, CAPTCHA o petición de inicio de sesión?
- T09-Q04 — Si se autorizara una revisión periódica, ¿qué día, hora y alcance tendría?
- T08-Q05 — ¿Qué evidencia humana demuestra la retirada o visibilidad de todos los anuncios relacionados?
- T09-Q05 / X-LIVE-002 — ¿Debe guardarse una URL opcional para navegación o auditoría y qué se hace si falla?

