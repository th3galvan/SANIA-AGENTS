---
rol: observabilidad
actualizado: PENDIENTE
---

# Plano operativo · OBSERVABILIDAD — SIN ENTREVISTAR

> **Este fichero está vacío a propósito y no se rellena de memoria ni "a ojo".** Es la salida
> escrita de la entrevista de arranque del rol (ADR-008), y mientras siga así:
> `<HARD-GATE>` **el rol OBSERVABILIDAD no mira nada y no informa de nada**.

## Cómo se llena (una vez, y ya)

1. Sesión nueva con el rol OBSERVABILIDAD. Un rol = una sesión: no se mezcla con la de
   construir.
2. Preguntar al usuario, en su idioma y **una por una** (ficha del rol en
   `docs/00-metodo/roles.md`):

   1. ¿Qué se vigila hoy, y con qué? (¿hay algo que avise si la aplicación se cae?)
   2. ¿Dónde están los logs? (si algo falla, ¿dónde se mira?)
   3. ¿Dónde se ven los errores? (¿alguien se entera cuando un usuario ve una pantalla rota?)
   4. ¿Qué significa para ti que «va bien»? (qué tiene que funcionar sí o sí, y a qué hora del día importa más)
   5. ¿A quién se avisa cuando algo se rompe, y cómo? (persona, canal, y si es de noche)
   6. ¿En qué etapa está el proyecto: local, red local (LAN) o internet (VPS)?

3. **Sobrescribir este fichero ENTERO** desde
   `docs/00-metodo/plantillas/plano-operativo.md` (`rol: observabilidad`), con las respuestas en
   palabras del usuario. De este texto no se conserva nada: no es una plantilla que rellenar
   hueco a hueco, es un cartel de "aquí todavía no hay nada".

Lo que el usuario no sepa pero exista, el rol lo DERIVA con evidencia y se lo confirma. Lo que NO exista no lo construye: es un hallazgo, y va a la sección «Lo que NO existe todavía» del plano.

Mientras tanto, cualquier informe de estado sería una opinión: sin saber qué se vigila y qué significa «va bien» para el usuario, no hay nada contra lo que comparar.
