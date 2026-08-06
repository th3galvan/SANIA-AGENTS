# Adopción de las carpetas anteriores

## Alcance y fuentes

- `C:\Projects\old1`: 198 ficheros útiles, 185.313.793 bytes. Contiene planos históricos, 16 actividades detalladas, entrevistas, evidencias, un Excel de costes y fotografías de producto.
- `C:\Projects\old2`: 82 ficheros útiles, 672.316 bytes. Contiene un workspace anterior, un mapa general más reciente y un repositorio de código vacío salvo por su README.
- Ninguna fuente contiene una aplicación implementada que se pueda compilar o ejecutar.

Las carpetas originales se han tratado en solo lectura y permanecen intactas.

## Inventario incorporado

- Mapa canónico: copia de `C:\Projects\old2\docs\02-flujos\planos\planos.json`.
- Fuentes históricas de `old1`: `docs/03-investigacion/fuentes/old1/docs/`, salvo la biblioteca de fotografías `Products/`, que el usuario decidió no incorporar por el momento.
- Copia de contraste de `old2`: `docs/03-investigacion/fuentes/old2/`.
- El documento legible `spec.md` se regeneró desde el mapa canónico; no se reutilizó como fuente maestra.
- Las 16 actividades históricas se convirtieron al esquema vigente en `docs/02-flujos/planos/actividades/`. Sus identificadores de reglas, episodios, recorridos, promesas, pruebas y calidad se renumeraron de forma única para todo SANIA; los identificadores antiguos permanecen anotados en el texto y en las fuentes íntegras.

## Comparación de los mapas

Ambos mapas describen SANIA, contienen 33 actividades y comparten propósito, actores y vocabulario. El mapa de `old2` es posterior y modifica cuatro bloques: actividades, datos, calidad y preguntas.

La diferencia funcional identificada añade una protección para los valores `b`, `i` y `r` presentes en enlaces de Wallapop: se conservan juntos como pista provisional, pero una ausencia, cambio o contradicción no permite unir operaciones automáticamente. Esta protección aparece también como `Q-21`. Por ser la versión posterior y más segura, `old2` queda como mapa canónico.

## Comandos y comprobaciones ejecutados

- Inventario recursivo de ambas carpetas, excluyendo `.git`, `.private`, `.runtime` y `worktrees`.
- Comparación estructural de los dos `planos.json` por bloques y por actividad.
- Validación del mapa de `old2` con `visor/validar.py`: `OK: planos válidos para perfil borrador (0 aviso(s)).`
- Validación inicial de los 16 planos de actividad de `old1`: los 16 fallaban el esquema vigente.
- Conversión de los 16 planos y validación posterior: 0 errores. Persisten 90 avisos de contenido (85 reglas todavía sin promesa comprobable, cuatro transiciones de estado por aclarar y una ficha sin dispositivo de acceso), coherentes con su estado `en entrevista`.
- Regeneración de `spec.md`: completada, 269 líneas.

## Estado de pruebas y código

No existe código de producto ni suite de pruebas en las fuentes. `old2/main/` solo contiene un README y `old1/codebase/main/` está vacío. Por tanto, no hay build, arranque ni pruebas de aplicación que ejecutar.

## Mapa de diferencias

| Elemento | Estado | Evidencia | Decisión |
|---|---|---|---|
| Mapa general de SANIA | Incorporado | `docs/02-flujos/planos/planos.json` | Se adopta la versión válida y más reciente de `old2`. |
| 33 actividades del negocio | Incorporadas en el mapa | bloque `actividades` del mapa canónico | Se mantiene el catálogo completo. |
| 16 actividades detalladas | Convertidas a borradores vigentes | `docs/02-flujos/planos/actividades/` | Pasan el esquema actual con 0 errores. Siguen `en entrevista` hasta resolver sus avisos de contenido y aprobarlas. |
| Entrevistas y evidencias | Incorporadas como fuentes | `docs/03-investigacion/fuentes/old1/docs/entrevistas/` y `evidencias/` | Servirán para contrastar cada actividad. |
| Excel de costes | Incorporado como fuente | `docs/03-investigacion/fuentes/old1/docs/costes_aliexpress.xlsx` | Pendiente de revisar cuando se abra la actividad financiera correspondiente. |
| Fotografías de productos | Excluidas por decisión del usuario | `C:\Projects\old1\docs\Products` | No se incorporan por el momento; permanecen intactas en `old1`. |
| Aplicación y pruebas | No implementadas | `old1/codebase/main/` vacío; `old2/main/README.md` | La construcción partirá de los planos una vez revisados. |

## Siguiente trabajo recomendado

Revisar las 16 actividades convertidas siguiendo el orden y las dependencias del mapa. Primero deben resolverse las reglas sin prueba, las cuatro transiciones de estado y el dispositivo pendiente; después se validará visualmente cada actividad antes de marcarla como especificada.
