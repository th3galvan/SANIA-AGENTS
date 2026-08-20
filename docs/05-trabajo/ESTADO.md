# ESTADO — ingeniería de requisitos en curso

## Posición actual

- **Fase**: definición de flujos; el diseño aún no está congelado.
- **Workspace**: creado desde el principio; los planos canónicos ya viven aquí.
- **Siguiente acción obligatoria**: entrevistar al usuario o, si la salta, proponer y completar todos los planos.

## Último cierre parcial

- **2026-08-19 — anuncios y Compra personal**: cada producto guarda sus imágenes, descripción, precio, precio máximo y mínimo aceptable en negociación; por defecto, precio y mínimo son iguales en Wallapop y Vinted, pero Vinted puede usar un precio de publicación superior sin cambiar el mínimo. Víctor decide arbitrariamente ese precio de Vinted por producto, sin fórmula ni recargo automático; cuando cambia el precio común, queda pendiente de revisión manual sin recalcularse ni crear tarea o recordatorio, SANIA avisa inmediatamente por Telegram y lo muestra en el dashboard. Las unidades idénticas sin etiqueta son intercambiables: la referencia es trazabilidad lógica y Víctor puede entregar cualquiera. SANIA mide el tiempo hasta la venta desde que recibe y guarda el enlace que confirma la publicación; corregirlo después no reinicia la medición. Enviar imágenes entrega las asociadas al producto. Su formato y cantidad se decidirán al definir su generación, con compatibilidad para Wallapop y Vinted. Cada producto mantiene como máximo un anuncio activo en Wallapop y otro en Vinted. Con dos o más unidades usan referencias distintas; con una sola comparten referencia. Víctor crea personalmente el anuncio y SANIA solo lo marca como publicado después de recibir su enlace; pulsar Anuncio creado sin enviarlo no cambia el estado. El enlace puede corregirse desde la ficha del anuncio en SANIA o mediante una acción de Telegram; al sustituirlo, SANIA elimina completamente el anterior y no lo conserva en el historial. Si SANIA cuestiona un enlace, Víctor puede enviar por mensaje el enlace correcto o pulsar El enlace es correcto; ambas acciones lo aceptan definitivamente y no vuelven a generar preguntas. Si recibe el mismo enlace por segunda vez, SANIA avisa y pide confirmación; un sí equivale a El enlace es correcto. Tras una venta con stock, SANIA crea otra tarea para la plataforma. Cancelar sugerencia desactiva nuevas propuestas para ese producto y plataforma; Volver a sugerir las reactiva y crea inmediatamente una tarea si hay stock y no existe allí un anuncio activo. Recordar más tarde conserva la tarea y la vuelve a presentar a las 18:00 del día siguiente según la hora local de SANIA. Mientras las sugerencias estén desactivadas, una publicación posterior se vincula manualmente. Al pasar un pedido a Compra personal, SANIA retira su stock y borra las tareas relacionadas.

## Regla de salida

No presentar para aprobación hasta pasar `validar.py --perfil revision` y `validar_web.py`; no congelar hasta la aprobación explícita del usuario.
