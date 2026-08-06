# Catálogo de correos transaccionales de Wallapop y Vinted

**Estado:** evidencia real anonimizada para consulta de agentes.  
**Fecha de extracción:** 04/08/2026.  
**Corpus:** seis archivos `.eml` aportados por Víctor.  
**Finalidad:** reconocer plantillas, extraer campos y decidir qué cambio de estado respalda cada correo sin tener que volver a abrir los mensajes originales.

Este catálogo resume evidencia; no sustituye los planos. Si una regla de este documento y los planos difieren, el agente debe señalar la contradicción y pedir que se actualicen los planos antes de construir.

## 1. Consulta rápida

| Necesidad | Dónde mirar |
|---|---|
| Saber si un correo cierra una venta | Secciones 3 y 5 |
| Reconocer una plantilla | Sección 3 |
| Saber qué campos se pueden extraer | Sección 3 |
| Preparar datos de prueba | Sección 4 |
| Identificar el `.eml` original | Sección 2, mediante su SHA-256 |
| Tratar un correo nuevo o dudoso | Sección 6 |
| Conocer lo que todavía no está demostrado | Sección 7 |

## 2. Origen, privacidad e inventario

Los originales estaban, en el momento de la extracción, en:

```text
C:\Users\visto\Downloads\CORREOS-SANIA-ENTREVISTA\
```

No se copiaron al repositorio porque contienen nombres, alias, dirección postal, enlaces privados e identificadores de transacción. Los nombres de varios archivos están dañados por una doble codificación; para localizarlos debe utilizarse el asunto normalizado y, sobre todo, la huella SHA-256.

| Evidencia | Plataforma | Asunto normalizado | Fecha de cabecera | SHA-256 | Hecho principal |
|---|---|---|---|---|---|
| EMAIL-001 | Vinted | `Has vendido un artículo en Vinted` | 29/07/2026 16:03:48 UTC | `8520b71de38472c8cd1edec48754857d4dbbf151709f13c1c53cd117ad375329` | Venta inicial; exige envío, no cierra la operación |
| EMAIL-002 | Vinted | `La transacción se ha completado` | 08/07/2026 13:38:34 UTC | `110bac77bda68bf3d576da31a53e32ff66e118e39efe413d4f6dd11b36a6c858` | Cierre con transacción e importes |
| EMAIL-003 | Vinted | `La transacción se ha completado` | 15/07/2026 19:58:19 UTC | `5c57b5b4e9d88f7f6d5039219c28ff823d5bde14a1cc5860e10b3352d5edc9c4` | Cierre con transacción e importes |
| EMAIL-004 | Vinted | `La transacción se ha completado` | 16/07/2026 14:55:25 UTC | `92a33b670c26181a2fde34ea66d9f773e4a46d92fd26e4751212fa7e414091b1` | Cierre con transacción e importes |
| EMAIL-005 | Wallapop | `Wallapop Envíos: tu paquete ha sido entregado :)` | 27/07/2026 17:50:57 UTC | `b1b26fe49278347568b209301f6c9d973c02ed444efc93f09f9abc8c65c154c0` | Entrega pendiente del OK del comprador; no es cierre económico |
| EMAIL-006 | Wallapop | `¡Venta confirmada! Sigue las instrucciones para enviar tu paquete` | 29/07/2026 07:53:37 UTC | `4fe587570304de089611c999fc2181a7e279c86a843d0d0199d3313629920506` | Venta confirmada y pendiente de envío |

Todos los mensajes tenían cabecera `Message-ID`. SANIA debe conservarla y tratarla como identificador único de la evidencia recibida, no como identificador de negocio de la venta.

## 3. Plantillas observadas y extracción

Antes de comparar, hay que decodificar MIME, eliminar espacios sobrantes y normalizar los saltos de línea del asunto. El asunto nunca basta por sí solo: se combina con el remitente y con marcas del cuerpo.

### VINTED-SALE-CREATED — venta inicial

**Reconocimiento observado**

```text
remitente = no-reply@vinted.es
asunto normalizado = Has vendido un artículo en Vinted
cuerpo contiene = ha comprado
cuerpo contiene = Transferiremos el pago del comprador
cuerpo contiene = Envía el pedido en los próximos 5 días
```

**Campos disponibles**

- Alias del comprador.
- Título del artículo.
- Precio del artículo.
- Plazo relativo de envío de cinco días.
- Enlaces de actuación en Vinted.

**Campos no observados**

- Número de transacción.
- Precio del envío.
- Importe transferido al saldo.
- Confirmación de cierre.

**Efecto respaldado:** detectar la venta, reservar la unidad que corresponda y crear la acción de preparación/envío. No cerrar la venta ni registrar un ingreso definitivo.

### VINTED-TX-COMPLETE — cierre de transacción

`VINTED-TX-COMPLETE` es el nombre interno utilizado por SANIA. La cadena literal `TX-COMPLETE` no aparece en los tres correos: el asunto real observado es `La transacción se ha completado`.

**Reconocimiento observado**

```text
remitente = no-reply@vinted.es
asunto normalizado = La transacción se ha completado
cuerpo contiene = se ha completado la transacción
cuerpo contiene = N.º de transacción:
cuerpo contiene = Fecha:
cuerpo contiene = Precio del artículo:
cuerpo contiene = Precio del envío:
cuerpo contiene = Transferencia a tu saldo Vinted
```

**Campos disponibles**

- Número de transacción.
- Fecha y hora comunicadas por Vinted.
- Título exacto del artículo.
- Precio del artículo.
- Precio del envío.
- Importe transferido al saldo Vinted.

**Efecto respaldado:** cerrar económicamente la venta de Vinted y registrar la transferencia al saldo una sola vez.

**Identidad e idempotencia:** el número de transacción es obligatorio y debe ser único. El título no identifica la operación: EMAIL-003 y EMAIL-004 tienen el mismo título base, pero transacciones e importes diferentes.

**Regla económica:** `Precio del envío` no es ingreso del vendedor. En los tres casos observados, la transferencia al saldo coincide con el precio del artículo y no incluye el envío.

### WALLAPOP-SALE-CONFIRMED — venta confirmada

**Reconocimiento observado**

```text
remitente = info@wallapop.com
asunto normalizado empieza por = ¡Venta confirmada!
cuerpo contiene = confirmación de tu venta
cuerpo contiene = Comprado por:
cuerpo contiene = Total
cuerpo contiene = Ver instrucciones de envío
cuerpo contiene = El dinero quedará congelado
```

**Campos disponibles**

- Alias abreviado del comprador.
- Título del anuncio o producto.
- Precio del artículo.
- Modalidad y coste de envío mostrados.
- Total.
- Fecha de compra en texto.
- Enlace a las instrucciones de envío.

**Efecto respaldado:** confirmar la venta, reservar la unidad y dejarla pendiente de preparación y depósito. No cerrar económicamente la venta: el propio correo dice que el dinero queda congelado.

La correlación mediante los enlaces y sus parámetros opacos se documenta en el [informe detallado de Wallapop](../informe_flujo_correos_wallapop.md). No debe atribuirse significado oficial a esos parámetros sin nueva evidencia.

### WALLAPOP-DELIVERED-PENDING-BUYER — paquete entregado

**Reconocimiento observado**

```text
remitente = info@wallapop.com
asunto normalizado empieza por = Wallapop Envíos: tu paquete ha sido entregado
cuerpo contiene = ¡Paquete entregado!
cuerpo contiene = confirme que todo está OK
cuerpo contiene = el dinero estará disponible en tu monedero
```

**Campos disponibles**

- Alias abreviado del comprador.
- Título del anuncio o producto.
- Precio del artículo.
- Modalidad y coste de envío mostrados.
- Total.
- Fecha de compra en texto.
- Enlace a los detalles.

**Efecto respaldado:** marcar el hecho logístico como entregado y mantener la venta abierta, pendiente del OK del comprador y de la liberación del dinero.

**Prohibición:** este correo no permite marcar la venta como cerrada ni el ingreso como definitivo.

## 4. Datos reales anonimizados para pruebas

| Evidencia | Plantilla | Precio del artículo | Precio del envío | Total o transferencia | Particularidad útil |
|---|---|---:|---:|---:|---|
| EMAIL-001 | Vinted venta inicial | 70,00 € | No consta | Pago futuro, sin importe final | Plazo expresado como «próximos 5 días» |
| EMAIL-002 | Vinted cierre | 17,00 € | 2,65 € | Transferencia 17,00 € | Primera muestra de cierre |
| EMAIL-003 | Vinted cierre | 25,00 € | 3,85 € | Transferencia 25,00 € | Comparte título base con EMAIL-004 |
| EMAIL-004 | Vinted cierre | 24,95 € | 4,99 € | Transferencia 24,95 € | Comparte título base con EMAIL-003 |
| EMAIL-005 | Wallapop entregado | 50,00 € | 0,00 € | Total 50,00 € | Expresa que todavía falta el OK del comprador |
| EMAIL-006 | Wallapop venta confirmada | 130,00 € | 0,00 € | Total 130,00 € | Dinero congelado y acción de envío pendiente |

Los datos personales, títulos completos, direcciones, enlaces y números reales de transacción se omiten a propósito. Para una prueba de integración que necesite el MIME exacto debe utilizarse una copia anonimizada creada durante la construcción, nunca el `.eml` personal dentro del repositorio.

## 5. Conclusiones que un agente puede utilizar

1. Vinted dispone de una plantilla final observada tres veces que permite cerrar una venta por su número de transacción.
2. El evento interno puede llamarse `VINTED_TX_COMPLETE`, pero no debe buscarse la cadena literal `TX-COMPLETE` en el correo.
3. El número de transacción de Vinted permite impedir que el mismo cierre se aplique dos veces.
4. Título, comprador e importe no deben utilizarse solos como identidad de la operación.
5. El primer correo de venta de Vinted no es evidencia de cierre.
6. La confirmación de venta de Wallapop no es evidencia de cierre económico.
7. La entrega de Wallapop tampoco es evidencia de cierre: deja expresamente pendiente el OK del comprador.
8. Ninguno de estos seis correos demuestra la liberación final del monedero de Wallapop.
9. Un correo reconocido puede actualizar únicamente los hechos y campos que contiene; no permite inventar estados intermedios ausentes.

## 6. Procedimiento ante un correo dudoso o nuevo

SANIA no debe modificar stock, venta ni dinero si ocurre cualquiera de estas situaciones:

- Remitente distinto del observado.
- Asunto desconocido o con indicadores de varias plantillas.
- Falta un campo marcado como obligatorio.
- Número de transacción ausente en un supuesto cierre Vinted.
- El evento haría retroceder el estado ya conocido.
- La fecha, el título o los importes contradicen la operación relacionada.
- El correo parece cancelación, devolución, disputa, incidencia o reembolso y no existe todavía una plantilla validada.

En esos casos debe crear una revisión pendiente, avisar por Telegram y esperar confirmación humana. Una revisión solo se convierte en incidencia cuando se confirma que existe un problema operativo que necesita seguimiento.

## 7. Huecos que este corpus no resuelve

- Correo o señal final de Wallapop tras el OK del comprador.
- Entrada final del dinero en el monedero de Wallapop y posibles movimientos que no sean ventas.
- Variantes de idioma o de plantilla del cierre de Vinted: solo se observó una plantilla española repetida tres veces.
- Cancelaciones antes o después del envío.
- Admisión por transportista, tránsito, intento fallido y extravío.
- Disputas, devoluciones, reembolsos y sustituciones.
- Retención autorizada de alias de compradores y demás datos personales.

Estos huecos deben seguir visibles como pendientes; no pueden cerrarse por inferencia.

## 8. Documentos relacionados

- [Informe detallado del flujo de correos de Wallapop](../informe_flujo_correos_wallapop.md).
- [Planos de registrar una venta confirmada](../actividades/registrar-venta-confirmada/planos.json).
- [Planos de cerrar una venta entregada](../actividades/cerrar-venta-entregada/planos.json).
- [Documento generado de registrar una venta confirmada](../especificaciones/02-flows/anuncios-y-ventas/registrar-venta-confirmada.md).
- [Documento generado de cerrar una venta entregada](../especificaciones/02-flows/preparacion-envio-y-posventa/cerrar-venta-entregada.md).
