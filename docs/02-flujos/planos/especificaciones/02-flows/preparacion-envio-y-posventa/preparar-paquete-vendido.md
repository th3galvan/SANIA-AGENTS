# Spec: Preparar el paquete vendido

Proyecto `sania-preparar-paquete-vendido`. Generado desde `planos.json` (la fuente de verdad): no editar a mano.

**Estado del diseño:** borrador · **modo:** mixto.

**Cobertura observada en el código actual:** no verificado.

## 1. Propósito

SANIA mantendrá la venta reservada y mostrará la unidad y el contenido documentado, pero Víctor revisará y preparará físicamente el paquete. La descripción final del anuncio es la referencia operativa de lo vendido: cualquier extra o lote acordado debe quedar reflejado allí antes de cerrar la oferta.

Cuando una venta quedó reservada, Víctor necesitó reconocer la unidad y comprobar en el anuncio final qué debía introducir en el paquete, incluidos los extras documentados, sin convertir una conversación aislada en la fuente definitiva.

Criterios de éxito:
- El paquete preparado coincidió con la descripción final del anuncio.
- Un extra acordado solo se trató como contenido vendido después de reflejarse en el anuncio.
- Preparar el paquete no se confundió con entregarlo al transportista ni con cerrar la venta.

## 2. Actores y vocabulario

- **Víctor · dirige el negocio, consulta SANIA, toma decisiones, confirma los hechos físicos, prepara y envía los paquetes y es el responsable por defecto de todas las incidencias**

- "descripción final del anuncio": versión pública del anuncio que debe reflejar exactamente el contenido vendido, incluidos los extras acordados
- "unidad reservada": unidad apartada desde que una plataforma comunica la venta; deja de contar como disponible aunque la operación todavía no haya terminado

## 3. El proceso (flujos)

La versión gráfica vive en el visor local del paquete (visor/servir.py).

### Víctor preparó el contenido documentado en el anuncio final [con la app · origen: usuario]

- [automático: código] SANIA mostró la plataforma, la venta, el título final con la referencia de tres caracteres y la unidad reservada.
- [persona] Víctor abrió manualmente el anuncio y, cuando fue necesario, la conversación para revisar qué se había vendido. · Víctor
- ⚑ Regla: ¿Se había acordado un extra o un lote?
    - si sí:
        - [persona] Víctor comprobó que el anuncio se había modificado para reflejar el contenido y el importe acordados antes de tratar el extra como vendido. · Víctor
        - ⚠ Excepción: ¿La descripción final seguía sin coincidir con el acuerdo?
            - si sí:
                - [automático: código] SANIA mantuvo la preparación bloqueada sin inventar el contenido del paquete; T05-Q03 dejó pendiente si la discrepancia abre una incidencia o ticket y cuál es su tratamiento.
                - aquí termina este camino
            - camino normal: no, continuó la preparación
        - …y vuelve al flujo
    - camino normal: no, se preparó lo descrito en el anuncio
- [persona] Víctor localizó visualmente la unidad en la caja bajo el escritorio, reunió el contenido descrito y cerró físicamente el paquete. · Víctor
- [automático: código] SANIA mantuvo la venta pendiente de admisión; el hecho de que el paquete estuviera preparado no probó su entrega al transportista.

## 4. Recorridos, requisitos y criterios de aceptación

El orden es el orden de entrega. El primero es el esqueleto: recorre el camino feliz de punta a punta.

(Pendiente.)

### Episodios reales que sustentan los requisitos

- En una venta ordinaria, Víctor revisa anuncio y conversación, localiza la unidad con ojos y manos y prepara físicamente el paquete antes de obtener y presentar el QR. [Migración: referencias históricas: E-LIVE-001, T01-Q01, T05-Q01]
- Cuando se acuerda un extra, Víctor pide una oferta que lo incluya y modifica el anuncio para que la descripción final documente exactamente lo enviado. [Migración: referencias históricas: E-LIVE-002, T01-Q07, T05-Q02]

## 5. Reglas de negocio

### G-61: El paquete coincide con el anuncio final

La descripción final del anuncio, no una conversación aislada, documenta el contenido vendido; un extra solo se prepara después de quedar reflejado en el anuncio. [Migración: identificador histórico G-LIVE-010; referencias históricas: D-LIVE-002, T01-Q07]

### G-62: El material de envío se obtiene manualmente

Víctor consulta manualmente desde el correo o la conversación las instrucciones, el transportista y el QR; SANIA no debe afirmar que los obtiene de la plataforma. [Migración: identificador histórico D-LIVE-003]

## 6. Estados

(Pendiente.)

## 7. Datos e integraciones

- Habla con **Telegram**: mostrar el contexto de la venta y avisar de una discrepancia sin sustituir la revisión física de Víctor
- Habla con **Wallapop y Vinted**: ser consultadas manualmente por Víctor; SANIA no lee conversaciones ni modifica anuncios

## 8. Superficie de uso

(Pendiente.)

## 9. Calidad y límites

(Pendiente.)

## 10. Fuera de alcance

- Leer automáticamente conversaciones o instrucciones dentro de Wallapop o Vinted.
- Crear, modificar o completar automáticamente el anuncio para documentar un extra.
- Embalar físicamente el producto.
- Crear un recordatorio genérico para preparar la unidad: Víctor lo rechazó en la entrevista.
- Dar por admitido o enviado un paquete únicamente porque se preparó.

## 11. Preguntas abiertas

Buzón del constructor: sus dudas se apuntan aquí, nunca se responden de palabra.

- [T01-Q06, T05-Q02] ¿Qué ocurrió en una venta real con varios productos o varias unidades y cómo se documentó todo el contenido?
- [T05-Q01] ¿Qué datos mínimos, además del título con referencia y la descripción final, necesita ver Víctor para localizar la unidad correcta?
- [T05-Q03] ¿Qué errores reales de preparación deben abrir una incidencia antes de entregar el paquete?
- [T04-Q02, X-LIVE-004] ¿Cómo se sostiene la identidad exacta de referencias distintas cuando las unidades idénticas no llevan etiqueta ni tienen rasgos diferenciadores?

