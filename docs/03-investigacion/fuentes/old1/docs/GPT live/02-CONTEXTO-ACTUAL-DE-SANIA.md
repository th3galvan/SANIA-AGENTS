# Contexto actual de SANIA

## Propósito

SANIA es la aplicación personal de Víctor para automatizar el registro y seguimiento de su negocio: compra productos en AliExpress y los vende en Wallapop y Vinted.

Cuando llega información nueva, SANIA debe actualizar pedidos, almacén, ventas, envíos y beneficios, pidiendo a Víctor únicamente las confirmaciones que requieren comprobar algo físicamente.

El objetivo es eliminar aproximadamente dos horas diarias de copia y registro manual.

## Éxito

- El registro manual de pedidos, almacén, ventas y beneficios baja a cero.
- Los cambios comunicados por correo se registran automáticamente.
- Cada venta realmente cerrada muestra su beneficio real.
- Un correo duplicado o fuera de orden no crea dos movimientos por el mismo hecho.

## Actor inicial

Solo existe un usuario inicial: Víctor. Dirige el negocio, consulta información, toma decisiones, confirma hechos físicos y prepara y envía los paquetes.

## Modelo operativo conocido

- Una compra de AliExpress contiene líneas, paquetes y unidades.
- Cada unidad física tiene una referencia interna y conserva su historia.
- Producto y variante son distintos de pedido, lote, unidad, anuncio, venta y envío.
- Víctor publica y modifica anuncios manualmente.
- Al publicar, envía a SANIA el enlace del anuncio para vincularlo con una unidad.
- La referencia interna nunca se escribe ni se oculta en el anuncio público.
- Una venta se reserva desde el primer correo reconocido.
- Primero se reserva la unidad exacta vinculada al anuncio.
- Si la exacta no está disponible, se usa por FIFO la unidad compatible más antigua.
- Si no hay unidad compatible, se abre una conciliación sin crear stock negativo ni ficticio.
- El precio final de la venta es provisional hasta que la operación termina realmente.
- Las correcciones conservan el valor anterior, el nuevo, el actor, la fecha y el motivo.

## Entradas y canales

- Una cuenta de Gmail con permiso de solo lectura recibe correos de AliExpress, Wallapop, Vinted, Correos e InPost.
- Telegram comunica decisiones, tareas físicas, excepciones y recordatorios.
- El texto libre de Telegram puede añadir contexto o abrir un ticket, pero no modifica directamente stock ni dinero.
- El Excel `costes_aliexpress.xlsx` es referencia del cálculo y del histórico hasta que se diseñe su importación.
- Está propuesta una lectura limitada de perfiles públicos de Wallapop y Vinted sin iniciar sesión, pero la excepción todavía necesita confirmación explícita.

## Datos compartidos

SANIA distingue y relaciona:

- producto y variante;
- pedido, línea y paquete de AliExpress;
- lote y unidad de inventario;
- historial de la unidad;
- anuncio;
- venta;
- envío;
- movimiento económico;
- ticket operativo;
- evento externo.

## Calidad ya decidida

- Un mismo hecho se aplica como máximo una vez.
- Todos los cambios de stock, estado y dinero son auditables.
- Nunca se crea stock negativo, una referencia ficticia ni una venta cerrada sin costes reales.
- Antes de guardar operaciones reales debe existir una restauración probada.
- En prototipo se admite como máximo una pérdida de 24 horas y una recuperación inferior a 4 horas.
- Con operaciones reales, PostgreSQL se copia al menos cada 6 horas y los archivos cada noche.

## Fuera de la primera versión

- Automatizar conversaciones o negociaciones dentro de Wallapop y Vinted.
- Crear, editar, republicar o cambiar precios mediante scraping o navegador.
- Etiquetas físicas QR o códigos de barras.
- Ubicación exacta por estantería o caja.
- Abrir o cerrar automáticamente disputas en AliExpress.
- Analizar conversaciones para decidir qué unidad se vendió.
- Automatizar devoluciones antes de observar un caso real.
- Resolver paquetes extraviados sin conocer el procedimiento real.
- Reasignar o republicar automáticamente anuncios tras una venta.

## Conflictos ya resueltos

- Las devoluciones quedan fuera del MVP hasta observar un caso real.
- La reserva se produce con el primer correo de venta, no después de una confirmación de Telegram.
- Se elige primero la unidad exacta y después FIFO.
- La referencia interna es privada aunque una tarea antigua propusiera publicarla.
- Pricing y conversaciones son evolución futura.
- El tiempo de venta de un anuncio sí interesa, pero no depende de scraping ni de una referencia pública.

## Actividades en entrevista

- Seguir pedidos de AliExpress.
- Confirmar la recepción.
- Dar entrada al almacén.
- Controlar stock y trazabilidad.
- Crear y mantener anuncios manuales.
- Comprobar productos publicados.
- Registrar una venta confirmada.
- Retirar anuncios sin stock.
- Preparar el paquete.
- Entregarlo al transportista.
- Seguir el envío.
- Cerrar la venta.
- Registrar movimientos económicos.
- Calcular beneficio.
- Atender alertas y confirmaciones.
- Resolver excepciones operativas.

## Temas aplazados

Selección y compra de productos, vigilancia de precios, oportunidades, conversaciones asistidas, republicación, competencia, rendimiento de anuncios, devoluciones, extravíos, importación financiera, cajas estándar, contenidos de afiliación, canal de desarrollo y formación.

