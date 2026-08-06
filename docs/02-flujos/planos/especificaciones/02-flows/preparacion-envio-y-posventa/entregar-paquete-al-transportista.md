# Spec: Entregar el paquete al transportista

Proyecto `sania-entregar-paquete-al-transportista`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

Víctor consultará manualmente en el correo, la conversación o las instrucciones de la plataforma qué transportista corresponde y obtendrá el QR para presentarlo en el punto de entrega. SANIA solo registrará la admisión cuando exista evidencia reconocida; no se ha acordado una confirmación manual sustitutiva ni un tiempo de espera.

Cuando el paquete estuvo preparado, Víctor necesitó consultar el transportista y el QR, llevarlo al punto indicado y distinguir la entrega física de una admisión demostrada.

Criterios de éxito:
- Víctor obtuvo manualmente el transportista y el QR desde la plataforma.
- SANIA no marcó el envío como admitido solo por estar preparado o porque Víctor hubiera salido a entregarlo.
- Una evidencia ausente, desconocida o contradictoria quedó pendiente o en ticket, sin una confirmación manual inventada.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### Víctor entregó el paquete y SANIA esperó una evidencia de admisión [con la app · origen: usuario]

- [persona] Víctor entró manualmente desde el correo o la conversación en las instrucciones de envío, identificó el transportista y obtuvo el QR. · Víctor
- [persona] Víctor llevó el paquete al punto indicado y presentó el QR que identifica el paquete. · Víctor
- ⚑ Regla: ¿Llegó un correo reconocido que demostraba la admisión y pudo relacionarse con este envío?
    - si no o la lectura fue dudosa:
        - [automático: código] SANIA mantuvo el envío pendiente de evidencia y dejó sin aplicar el correo desconocido, incompleto o contradictorio; T02-Q08 decidirá si además abre un ticket. No pidió ni aplicó un OK manual no acordado.
        - aquí termina este camino
    - camino normal: sí, SANIA registró la admisión y conservó la evidencia
- [automático: código] SANIA aplicó el hecho de admisión una sola vez y separó su estado del seguimiento y de la entrega posterior al comprador.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- En el caso ordinario, Víctor abrió las instrucciones desde el correo o la conversación, vio la empresa y el QR, preparó el paquete y presentó ese QR en el punto de entrega. [Migración: referencias históricas: E-LIVE-001, T01-Q01, T05-Q04]

## 5. Reglas de negocio

### G-59: Transportista y QR se obtienen manualmente

Víctor consulta las instrucciones en la plataforma y presenta el QR; SANIA no actúa dentro de Wallapop o Vinted ni presupone el transportista. [Migración: identificador histórico D-LIVE-003]

### G-60: Una admisión se aplica una sola vez

Reprocesar la misma evidencia externa no duplica el evento ni hace avanzar dos veces el envío. [Migración: identificador histórico G-LIVE-001; referencias históricas: T02-Q05]

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

| Cosa | Qué se guarda | De dónde viene |
|---|---|---|
| instrucciones de entrega y QR | venta y paquete relacionados cuando puedan demostrarse, transportista, origen consultado manualmente, QR o referencia de entrega cuando se aporte; todavía no se ha definido si SANIA conserva el valor ni durante cuánto tiempo, clasificación explícita del QR como dato sensible y temporal, retención, eliminación y permisos todavía pendientes | app, web, correo o conversación consultados manualmente por Víctor; SANIA no tiene autorizada su lectura automática |

- Habla con **Gmail**: recibir y conservar correos reconocidos de admisión
- Habla con **Telegram**: avisar a Víctor de un ticket o una evidencia pendiente, sin sustituirla por una confirmación no acordada
- Habla con **Wallapop, Vinted y transportistas**: ser consultados manualmente por Víctor para obtener instrucciones, transportista y QR

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Transportar físicamente el paquete.
- Leer automáticamente instrucciones o QR dentro de Wallapop o Vinted.
- Marcar el paquete como admitido solo porque estaba preparado o Víctor indicó que lo había llevado.
- Pedir una confirmación manual de admisión mientras no se definan sus casos y prioridad frente al correo.
- Crear recordatorios con un umbral temporal no acordado.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T02-Q04, T05-Q04] ¿Qué plantilla y qué identificadores demuestran la admisión en Correos, InPost y cada transportista real?
- [T05-Q05] ¿En qué casos exactos se aceptará una confirmación manual de admisión, si se acepta alguno?
- [T05-Q06] ¿Qué prioridad tendrá una confirmación manual si después llega un correo contradictorio?
- [T05-Q07] ¿Cuánto tiempo se espera antes de recordar una admisión todavía sin evidencia?
- [T02-Q08] ¿Qué estado y ticket se aplican a un correo de admisión desconocido o imposible de relacionar?
- [T05-Q04] El QR es sensible y temporal: ¿cuánto tiempo se conserva, cuándo se elimina y quién puede verlo?

