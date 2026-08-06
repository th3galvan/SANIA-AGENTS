# Formato del informe de devolución

El informe debe ser Markdown y respetar esta estructura.

## 0. Identificación

- Proyecto: SANIA.
- Origen del contexto: commit `024c8e7`.
- Fecha de la entrevista.
- Duración aproximada si se conoce.
- Temas tratados.
- Temas no tratados.

## 1. Resumen ejecutivo

Cinco a diez líneas con lo nuevo aprendido, las decisiones más importantes y el principal bloqueo.

## 2. Cobertura del guion

Tabla con:

| ID | Estado | Evidencia breve | Falta |
|---|---|---|---|

Incluir todos los `Tnn-Qnn` tratados o mencionados. Los no tratados pueden agruparse por tema.

## 3. Decisiones confirmadas

Usar identificadores `D-LIVE-001`, `D-LIVE-002`, etc.

Para cada decisión:

- decisión exacta;
- tema del guion;
- motivo o ejemplo que la sostiene;
- alcance: primera versión o futuro;
- dato, flujo o regla actual que podría cambiar.

## 4. Casos reales

Usar identificadores `E-LIVE-001`, `E-LIVE-002`, etc.

Para cada caso:

- situación inicial;
- producto, plataforma y participantes;
- hecho que lo inició;
- pasos en orden;
- datos reales disponibles;
- resultado;
- qué fue normal o raro;
- qué debería hacer SANIA.

No anonimizar los nombres de productos, plantillas o cifras que Víctor quiera usar como prueba.

## 5. Reglas de negocio

Usar identificadores `G-LIVE-001`, `G-LIVE-002`, etc.

Redactar cada regla como condición y resultado. Indicar si es confirmada, provisional o contradice una regla actual.

## 6. Datos y vocabulario

### Datos nuevos o modificados

Para cada dato:

- nombre;
- para qué se usa;
- de dónde sale;
- cuándo aparece;
- si es obligatorio;
- quién puede verlo;
- cuánto tiempo se conserva, si se decidió.

### Términos

Definir cualquier palabra usada con un significado especial.

## 7. Estados y transiciones

Para cada entidad afectada, indicar:

- estado de origen;
- hecho o persona que provoca el cambio;
- estado de destino;
- qué ocurre si el cambio falla;
- si se puede corregir y cómo conserva la historia.

## 8. Personas, permisos, avisos y tiempos

- quién puede hacer cada acción;
- qué requiere aprobación o segunda confirmación;
- quién recibe cada aviso;
- canal;
- momento y repetición;
- qué ocurre sin respuesta.

## 9. Excepciones y protecciones

Incluir duplicados, hechos fuera de orden, contradicciones, falta de stock, errores físicos, caídas, correos desconocidos y cualquier situación rara mencionada.

## 10. Contradicciones con los planos actuales

Usar identificadores `X-LIVE-001`, `X-LIVE-002`, etc.

Para cada contradicción:

- versión del contexto actual;
- nueva afirmación;
- evidencia;
- si Víctor confirmó expresamente que sustituye a la anterior;
- pregunta necesaria para resolverla.

No resolver la contradicción por cuenta propia.

## 11. Preguntas que siguen abiertas

Lista priorizada con su `Tnn-Qnn`, por qué sigue abierta y qué ejemplo o documento permitiría cerrarla.

## 12. Temas aplazados

Indicar qué temas decidió posponer Víctor y hasta qué hecho o momento.

## 13. Fragmentos literales útiles

Incluir únicamente frases breves de Víctor que aclaren una regla, un término o un caso. No reconstruir citas de memoria.

## 14. Nota para Codex

Cerrar con:

- tres cambios de planos que parecen necesarios;
- tres zonas de mayor incertidumbre;
- archivos, correos o ejemplos reales que Víctor debería aportar;
- declaración explícita de que el informe es materia prima y no modifica por sí solo los planos.

