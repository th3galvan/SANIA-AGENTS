# Informe del flujo de correos de venta de Wallapop

**Periodo analizado:** del 28 de mayo al 28 de julio de 2026.  
**Cobertura:** todos los correos del remitente Wallapop, incluyendo mensajes archivados en Papelera.

## 1. Resumen ejecutivo

Se localizaron **27 correos procedentes de Wallapop**:

- **12 correos transaccionales** relacionados con ventas como vendedor.
- **15 correos descartados**:
  - 6 sobre eliminación de anuncios.
  - 3 de publicidad o promociones.
  - 3 de seguridad o gestión de cuenta.
  - 3 relacionados con compras realizadas como comprador.

Dentro de los 12 correos válidos se identificaron:

- **4 tipos funcionales de correo**.
- **5 patrones de asunto**, porque el recordatorio de envío tiene dos variantes.
- **Ningún duplicado idéntico** entre los correos transaccionales.
- Varias repeticiones de la misma plantilla para ventas distintas.

Los correos transaccionales utilizan siempre `info@wallapop.com`. `news@news.wallapop.com` se utiliza para marketing, pero `info@wallapop.com` también envía seguridad, gestión de anuncios y compras, por lo que **el remitente no permite clasificar por sí solo el correo**.

### Consistencia del flujo

- **Alta consistencia** en asuntos, textos clave, orden de los bloques y enlaces.
- **Alta fiabilidad** para clasificar los cuatro tipos encontrados mediante reglas deterministas.
- **Cobertura incompleta del ciclo logístico**: no se encontraron correos de admisión por el transportista, tránsito, reparto, confirmación final del comprador, valoración ni liberación efectiva del pago.
- Los enlaces de cada operación contienen referencias opacas que permanecen estables entre los correos de una misma venta, lo que permite correlacionarlos.

---

## 2. Tabla de tipos de correo

| Orden habitual | Tipo de correo | Patrón del asunto | Palabras clave | Estado anterior | Estado resultante | Acción requerida | Frecuencia | Fiabilidad |
|---:|---|---|---|---|---|---|---:|---|
| 1 | Nueva venta pendiente de configurar | `¡Has hecho una venta! Selecciona un método de envío para continuar` | `nueva venta`, `Selecciona un método de envío`, `Comprado por`, `Fecha de compra` | Venta no registrada | Venta detectada, pendiente de seleccionar envío | Seleccionar método de envío | 4 | Alta |
| 2 | Recordatorio de envío pendiente | `Recordatorio: Selecciona un método de envío para tu venta` o `Mañana termina el plazo para hacer tu envío` | `Tienes 5 días`, `Mañana termina el plazo`, `Selecciona un método de envío` | Pendiente de seleccionar envío | Sin cambio de estado; aumenta la urgencia | Seleccionar envío antes del plazo | 2 | Alta |
| 3 | Venta confirmada, pendiente de envío | `¡Venta confirmada! Sigue las instrucciones para enviar tu paquete` | `confirmación de tu venta`, `Ver instrucciones de envío`, `Total`, `Enviar desde un punto de entrega` | Pendiente de seleccionar envío | Venta confirmada, pendiente de depositar el paquete | Preparar y enviar el paquete | 3 | Alta |
| 4 | Paquete entregado, pendiente del comprador | `Wallapop Envíos: tu paquete ha sido entregado :)` | `¡Paquete entregado!`, `confirme que todo está OK`, `dinero estará disponible` | Estado logístico anterior no determinado | Entregado al comprador, pendiente de confirmación | Ninguna acción explícita del vendedor | 3 | Alta |

---

## 3. Descripción detallada de cada tipo

### 3.1. Nueva venta pendiente de seleccionar envío

**Naturaleza:** cambio real de estado y acción requerida al vendedor.

Ejemplo anonimizado:

> Has hecho una nueva venta. Selecciona un método de envío para continuar.  
> Comprado por: `[COMPRADOR]`  
> `[PRODUCTO]`  
> Fecha de compra: `[FECHA]`

El correo incluye comprador, producto, fecha de compra y un enlace con el texto `Seleccionar método de envío`. No incluye importe, transportista, seguimiento ni código de envío.

#### Datos disponibles

| Dato | Disponibilidad |
|---|---|
| Identificador explícito de venta | No |
| Referencias opacas en el enlace | Sí |
| Producto o anuncio | Sí |
| Comprador | Sí, nombre o alias |
| Importe | No |
| Transportista | No |
| Seguimiento | No |
| Etiqueta o código de envío | No |
| Fecha de compra | Sí |
| Enlace a la operación | Sí |
| Estado logístico | No |

**Estado recomendado:** `PENDING_SHIPPING_METHOD`.

---

### 3.2. Recordatorio de envío pendiente

Es el mismo estado funcional con dos variantes.

#### Variante A: recordatorio inicial

Asunto:

`Recordatorio: Selecciona un método de envío para tu venta`

Texto característico:

- `Tienes 5 días para enviar tu paquete`.
- `Selecciona un método de envío para continuar`.

No representa un cambio real. Confirma que la operación continúa pendiente y que existe una acción del vendedor.

#### Variante B: último día próximo

Asunto:

`Mañana termina el plazo para hacer tu envío`

Texto característico:

- `Mañana termina el plazo para enviar tu paquete`.
- `envía el paquete antes de que acabe el día`.

Tampoco cambia el estado. Debe actualizar la urgencia o fecha límite, no sustituir el estado por uno nuevo.

#### Datos disponibles

- Comprador.
- Producto.
- Fecha de compra.
- Enlace a la operación.
- Plazo expresado en lenguaje natural.

No incluye importe, fecha límite estructurada, transportista, seguimiento ni etiqueta.

**Estado recomendado:** conservar `PENDING_SHIPPING_METHOD` y actualizar:

- `action_required = SELECT_SHIPPING_METHOD`
- `shipping_deadline_warning = true`
- `deadline_severity = REMINDER` o `FINAL_WARNING`

No debe crearse un estado independiente llamado “quedan cinco días”.

---

### 3.3. Venta confirmada, pendiente de envío

**Naturaleza:** cambio real de estado y acción requerida.

Aparece después de seleccionar el método de envío. Contiene:

- Confirmación de la venta.
- Comprador.
- Producto.
- Importe.
- Coste del método de envío para el vendedor.
- Total.
- Fecha de compra.
- Método genérico: `Enviar desde un punto de entrega`.
- Enlace `Ver instrucciones de envío`.

#### Datos disponibles

| Dato | Disponibilidad |
|---|---|
| Identificador explícito | No |
| Referencias del enlace | Sí |
| Producto | Sí |
| Comprador | Sí |
| Importe | Sí |
| Coste de envío para vendedor | Sí |
| Total | Sí |
| Método de envío | Sí, genérico |
| Transportista | No |
| Seguimiento | No |
| Etiqueta o código | No aparece en el correo |
| Fecha de compra | Sí |
| Enlace | Sí |

#### Variantes detectadas

Una versión incluye recomendaciones generales sobre peso y tamaño. Otra añade instrucciones de embalaje y advierte de posibles disputas. La estructura transaccional es la misma.

**Estado recomendado:** `SALE_CONFIRMED_PENDING_DROPOFF`.

---

### 3.4. Paquete entregado, pendiente de confirmación del comprador

**Naturaleza:** cambio real de estado logístico.

Texto característico:

> ¡Paquete entregado!  
> Cuando `[COMPRADOR]` confirme que todo está OK, el dinero estará disponible en tu monedero.

Incluye comprador, producto, importe, total, fecha de compra y enlace a los detalles. No incluye transportista ni código de seguimiento.

Este correo **no permite marcar la venta como completada ni el pago como liberado**. Demuestra expresamente que, después de la entrega, todavía se espera la confirmación del comprador.

**Estado recomendado:**

- `current_status = DELIVERED_PENDING_BUYER_CONFIRMATION`
- `buyer_confirmation = PENDING`
- `payment_status = HELD_PENDING_BUYER_CONFIRMATION`

No debe utilizarse `COMPLETED` ni `PAYMENT_RELEASED`.

---

## 4. Flujo completo observado

### Flujo normal respaldado por los correos

```text
Venta no registrada
→ Correo “Has hecho una venta”
→ Pendiente de seleccionar método de envío
→ El vendedor selecciona el método
→ Correo “Venta confirmada”
→ Venta confirmada, pendiente de depositar el paquete
→ [No se reciben correos intermedios de transporte]
→ Correo “Paquete entregado”
→ Entregado, pendiente de confirmación del comprador
→ Estado posterior no determinado
```

En dos operaciones se observa claramente la secuencia:

```text
Nueva venta → Venta confirmada → Paquete entregado
```

Los parámetros del enlace de la operación permanecen iguales en los distintos correos de la misma venta, lo que permite correlacionarlos.

### Venta pendiente sin configurar

```text
Nueva venta
→ Pendiente de seleccionar envío
→ Recordatorio: quedan cinco días
→ Sigue pendiente de seleccionar envío
→ Aviso: mañana termina el plazo
→ Sigue pendiente, acción urgente
→ Resultado posterior no determinado
```

No se encontró un correo que confirme automáticamente la cancelación de esta venta después del vencimiento.

### Cancelación

**No determinada para ventas como vendedor.**

Se encontró una cancelación de una compra realizada por el usuario como comprador, pero no debe utilizarse para definir el flujo de ventas del vendedor.

### Incidencia de transporte

**No determinada.**

No se localizaron correos sobre:

- Retrasos.
- Paquete perdido.
- Dirección incorrecta.
- Imposibilidad de entrega.
- Daños.
- Incidencia abierta con el transportista.

### Devolución

**No determinada.**

No se encontraron correos de devolución iniciada, devolución en tránsito ni paquete devuelto.

### Problema o rechazo del comprador

**No determinado.**

El correo de entrega menciona indirectamente que el comprador debe confirmar que todo está correcto, pero no se encontraron disputas, rechazos o reclamaciones.

### Pago retenido o pendiente

El dinero figura como congelado durante la operación y continúa pendiente después de la entrega hasta que el comprador confirme. No se encontró ningún correo de liberación o recepción del pago.

---

## 5. Estados obligatorios, opcionales y omitidos

### Estados principales observados

1. `PENDING_SHIPPING_METHOD`
2. `SALE_CONFIRMED_PENDING_DROPOFF`
3. `DELIVERED_PENDING_BUYER_CONFIRMATION`

Son los únicos estados de negocio respaldados directamente por correos.

### Eventos opcionales

- Recordatorio de cinco días.
- Recordatorio de último día.

Son notificaciones repetibles que no cambian el estado.

### Estados que pueden omitirse en el correo

No aparecen correos específicos para:

- Etiqueta disponible.
- Entregado al transportista.
- En tránsito.
- En reparto.
- Confirmación del comprador.
- Venta completada.
- Pago liberado.

Esto significa que el correo de entrega puede llegar directamente después del de venta confirmada.

### Correos fuera de orden

No se observó ningún correo que llegara realmente fuera de orden. Sí existen entregas cuyos correos iniciales no están dentro del periodo o no se conservaron, por lo que una revisión histórica puede encontrar una entrega sin sus eventos anteriores.

### Eventos repetibles

- Recordatorios de envío.
- Diferentes variantes del mismo recordatorio.
- La misma plantilla para operaciones distintas.

La deduplicación debe hacerse por `source_email_id` y referencia de operación, no únicamente por asunto.

---

## 6. Reglas propuestas para la automatización

### Preclasificación por remitente

| Regla | Resultado |
|---|---|
| `from = news@news.wallapop.com` | Excluir como marketing |
| `from = info@wallapop.com` | Continuar clasificando por asunto y cuerpo |
| Otro remitente | No clasificar automáticamente como Wallapop |

### Reglas deterministas

#### `SALE_CREATED`

```text
sender == info@wallapop.com
AND subject == "¡Has hecho una venta! Selecciona un método de envío para continuar"
AND body contiene "Has hecho una nueva venta"
AND body contiene "Selecciona un método de envío"
AND existe enlace cuyo destino contiene "/delivery/timeline"
```

Resultado:

```text
PENDING_SHIPPING_METHOD
```

#### `SHIPPING_METHOD_REMINDER`

```text
sender == info@wallapop.com
AND (
    subject empieza por "Recordatorio: Selecciona un método de envío"
    OR subject == "Mañana termina el plazo para hacer tu envío"
)
AND body contiene "Selecciona un método de envío"
```

Resultado:

- No cambiar `current_status`.
- Crear evento informativo.
- Actualizar acción pendiente y urgencia.

#### `SALE_CONFIRMED`

```text
sender == info@wallapop.com
AND subject empieza por "¡Venta confirmada!"
AND body contiene "confirmación de tu venta"
AND body contiene "Ver instrucciones de envío"
AND body contiene "Total"
```

Resultado:

```text
SALE_CONFIRMED_PENDING_DROPOFF
```

#### `DELIVERED_PENDING_BUYER_CONFIRMATION`

```text
sender == info@wallapop.com
AND subject empieza por "Wallapop Envíos: tu paquete ha sido entregado"
AND body contiene "¡Paquete entregado!"
AND body contiene "confirme que todo está OK"
AND body contiene "dinero estará disponible"
```

Resultado:

```text
DELIVERED_PENDING_BUYER_CONFIRMATION
```

### Correlación de la operación

Los enlaces están envueltos por un dominio de seguimiento de AWS, pero contienen como destino una URL similar a:

```text
www.wallapop.com/delivery/timeline?b=[VALOR]&i=[VALOR]&r=[UUID]
```

Los valores `b`, `i` y `r` se mantienen entre los correos de la misma operación.

Procedimiento recomendado:

1. Extraer el `href`.
2. Decodificar el destino incluido dentro del enlace de seguimiento.
3. Comprobar que el destino contiene `/delivery/timeline`.
4. Extraer `b`, `i` y `r`.
5. Guardar los tres valores sin asumir qué representa cada uno.
6. Utilizar provisionalmente la tupla `(b, i, r)` como clave externa de correlación.

No debe suponerse que `r` es oficialmente el identificador de pedido: esa semántica no aparece explicada en los correos.

### Extracción del cuerpo

Usar etiquetas visibles, no posiciones absolutas:

- Texto posterior a `Comprado por:`.
- Siguiente bloque como título del producto.
- Valor posterior a `Fecha de compra:`.
- Valor anterior o posterior a `Total`.
- Texto `Enviar desde un punto de entrega`.
- Anchor text `Seleccionar método de envío`, `Ver instrucciones de envío` o `Ver detalles`.

### Necesidad de IA

Los cuatro tipos encontrados **no necesitan IA**.

La IA o revisión manual solo debería utilizarse cuando:

- El asunto no coincide con ninguna plantilla conocida.
- Falta el enlace `/delivery/timeline`.
- No puede extraerse la referencia de operación.
- Aparecen indicadores de más de un tipo en el mismo mensaje.
- La transición implicaría retroceder el estado.
- La fecha del cuerpo contradice otros eventos.
- Se detecta una nueva plantilla de incidencia, devolución o pago.

---

## 7. Modelo recomendado para la base de datos

### Tabla de estado actual: `wallapop_orders`

```text
id
external_reference_b
external_reference_i
external_reference_r
listing_title_snapshot
buyer_alias
purchase_date_raw
purchase_date_normalized
amount
shipping_method
current_status
status_updated_at
buyer_confirmation_status
payment_status
action_required
action_deadline
deadline_severity
requires_manual_review
created_at
updated_at
```

No utilizar el nombre del comprador o el producto como identificador de la operación.

### Historial: `wallapop_order_events`

```text
id
order_id
source_email_id
email_type
sender
subject
email_received_at
event_date_raw
previous_status
resulting_status
is_state_change
is_informational
requires_seller_action
classification_confidence
template_version
extracted_data
idempotency_hash
created_at
```

`source_email_id` debe tener restricción `UNIQUE`.

### Datos logísticos

```text
order_id
shipping_provider
tracking_number
shipping_label_code
shipping_method
carrier_status
delivered_at
```

En los correos analizados, los tres primeros campos quedarían en `NULL`.

### Incidencias

```text
id
order_id
incident_type
incident_status
detected_at
resolved_at
source_email_id
details
requires_manual_review
```

No existen ejemplos suficientes para crear valores cerrados de `incident_type`.

### Acciones pendientes

```text
id
order_id
action_type
action_status
due_at
priority
created_from_event_id
completed_at
```

Acciones respaldadas por los correos:

- `SELECT_SHIPPING_METHOD`
- `PREPARE_AND_DROPOFF_PACKAGE`

### Separación conceptual

- `wallapop_orders`: fotografía actual.
- `wallapop_order_events`: historial inmutable.
- `extracted_data`: contenido obtenido del mensaje.
- `incidents`: ramas problemáticas.
- `pending_actions`: actuaciones que debe realizar el usuario.

---

## 8. Casos ambiguos y limitaciones

### No existe un identificador explícito de pedido

Los correos no muestran un campo denominado “pedido”, “venta” o “transacción”. Solo existe la referencia incluida en la URL.

### Inconsistencia crítica en las fechas

En una misma operación:

- El correo inicial indica `1/7/26`.
- El correo de entrega indica `7/1/26`.
- Ambos contienen las mismas referencias `b`, `i` y `r`.

Esto demuestra que el formato de fecha del cuerpo no es completamente fiable y puede invertir mes y día.

La fecha debe almacenarse en dos campos:

- `purchase_date_raw`
- `purchase_date_normalized`

Cuando haya ambigüedad, debe priorizarse la referencia de operación y marcarse `requires_manual_review = true`.

### Información no incluida de forma consistente

- Importe: ausente en nueva venta y recordatorios.
- Transportista: ausente en todos.
- Seguimiento: ausente en todos.
- Etiqueta o código de envío: ausente en todos.
- Fecha límite exacta: expresada únicamente como texto relativo.
- Identificador del anuncio: no se presenta explícitamente.
- Estado de pago final: no disponible.
- Confirmación efectiva del comprador: no disponible.

### Diferencias entre tipos de envío

Solo aparece `Enviar desde un punto de entrega`. No existen suficientes ejemplos para definir reglas para:

- Recogida a domicilio.
- Entrega en persona.
- Envíos voluminosos.
- Otros operadores o modalidades.

### Riesgos de actualización incorrecta

1. Marcar la venta como completada al recibir el correo de entrega.
2. Interpretar un recordatorio como cambio de estado.
3. Relacionar operaciones por producto o comprador.
4. Interpretar la fecha con un formato fijo.
5. Utilizar únicamente `info@wallapop.com` como criterio.
6. Sobrescribir un estado avanzado cuando llegue un correo antiguo.
7. Confundir compras del usuario con ventas.
8. Descartar mensajes históricos por estar en Papelera.

### Casos que deben quedar pendientes de revisión

- Correo transaccional sin enlace de operación.
- Parámetros `b`, `i` o `r` ausentes.
- Fecha ambigua que afecte a la identificación.
- Dos pedidos posibles con producto y comprador similares.
- Evento que implique regresión de estado.
- Nueva plantilla de cancelación, devolución, incidencia o pago.
- Entrega sin una operación previamente registrada.

---

## 9. Conclusión técnica

El correo permite automatizar con seguridad tres estados principales y un evento informativo:

```text
PENDING_SHIPPING_METHOD
→ SALE_CONFIRMED_PENDING_DROPOFF
→ DELIVERED_PENDING_BUYER_CONFIRMATION
```

Los recordatorios deben registrarse como eventos y acciones pendientes, no como estados. El correo no cubre suficientemente el transporte intermedio ni el cierre financiero, por lo que la base de datos deberá mantener esas fases como **no determinadas** hasta obtener evidencia adicional de Wallapop, del transportista o de futuros correos.
