# GUÍA — así funciona «SANIA» (sin tecnicismos)

La entrevista terminó y este es tu taller, ya montado. No necesitas saber
programar: tú diriges, los agentes construyen, y este sistema los mantiene a raya.

## Qué hay en esta carpeta

- `docs/` — el pensamiento del proyecto: qué es, cómo funciona tu negocio (lo que
  contaste en la entrevista), los arreglos pendientes (`docs/bugs/`) y el trabajo en
  marcha (`docs/05-trabajo/ESTADO.md` te dice dónde estás).
- `main/` — el código de tu aplicación. No se toca a mano: para eso están los agentes.
- `worktrees/` — las mesas de trabajo temporales donde construyen los agentes.
- `.private/` — tus secretos y contraseñas. Nunca sale de tu ordenador.
- `setup.py` — si te llevas esto a otro ordenador, lo ejecutas y se monta solo.

## Tú solo haces 3 cosas

1. **Pides**: abre tu agente en esta carpeta y cuéntale qué quieres ("quiero que se
   pueda…"). Él escribe el contrato EN TU IDIOMA.
2. **Apruebas leyendo**: te enseña el contrato — qué pasará cuando esté hecho, con
   ejemplos de tu negocio. Lo corriges y das el OK. Sin tu OK no se construye.
3. **Pruebas usando**: cuando digan "hecho", abres tu aplicación y compruebas con tus
   ejemplos reales. ¿Sí? Siguiente. ¿No? Di "esto no es lo que pedí" y se convierte
   en un arreglo con su prueba.

## Qué pasa por detrás

Cada petición tuya se convierte en: contrato aprobado por ti → un agente constructor
trabaja en una mesa aparte escribiendo PRIMERO las pruebas que deben cumplirse →
otro agente distinto revisa el trabajo → las pruebas corren solas (en GitHub si el
proyecto tiene CI; los creados de cero la traen, los adoptados la ganan en su
roadmap) → solo con todo en verde se incorpora al código → la mesa se recoge sola.
Nada entra sin
pruebas y sin revisión. Tu palabra final siempre es USAR la aplicación.

## Los seguros que te protegen

- Los contratos que juzgan el trabajo viven FUERA del alcance de quien construye.
- Nadie construye sin contrato escrito (hay un guardián automático).
- Cada error arreglado deja una prueba permanente: ese error no vuelve.
- Producción y servicios externos: solo lectura; los cambios de verdad te los piden.
- Secretos y datos sensibles: en `.private/`, que jamás sale de tu ordenador.

## Si tu negocio cambia

Vuelve a la herramienta de la entrevista (modo iteración): tus planos viven en
`docs/02-flujos/planos/`. De ahí se regenera la documentación — nunca a mano.
