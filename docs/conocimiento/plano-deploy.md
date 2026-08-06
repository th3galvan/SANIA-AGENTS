---
rol: deploy
actualizado: PENDIENTE
---

# Plano operativo · DEPLOY — SIN ENTREVISTAR

> **Este fichero está vacío a propósito y no se rellena de memoria ni "a ojo".** Es la salida
> escrita de la entrevista de arranque del rol (ADR-008), y mientras siga así:
> `<HARD-GATE>` **el rol DEPLOY no toca ninguna máquina**.

## Cómo se llena (una vez, y ya)

1. Sesión nueva con el rol DEPLOY. Un rol = una sesión: no se mezcla con la de
   construir.
2. Preguntar al usuario, en su idioma y **una por una** (ficha del rol en
   `docs/00-metodo/roles.md`):

   1. ¿Dónde corre esto hoy, y dónde debería correr? (tu ordenador, un equipo de la oficina, internet)
   2. ¿Qué etapas hay? (¿existe un sitio donde probar antes de que lo usen los demás, o solo hay uno y es el bueno?)
   3. ¿Cómo se despliega ahora? (qué haces exactamente para que un cambio llegue a la gente)
   4. ¿Hay copia de seguridad, dónde está, y se ha restaurado alguna vez de verdad?
   5. ¿Quién da el OK antes de que algo llegue a producción? (nombre de persona)
   6. ¿Qué pasa si hay que volver atrás? (cómo se deshace y cuánto se tarda)

3. **Sobrescribir este fichero ENTERO** desde
   `docs/00-metodo/plantillas/plano-operativo.md` (`rol: deploy`), con las respuestas en
   palabras del usuario. De este texto no se conserva nada: no es una plantilla que rellenar
   hueco a hueco, es un cartel de "aquí todavía no hay nada".

Si el usuario no ha desplegado NUNCA nada, estas preguntas no tienen respuesta y no se le puede pedir que decida a ciegas: se empieza por `docs/00-metodo/runbooks/primer-despliegue.md`, que pregunta por su negocio y deduce lo técnico.

Mientras tanto, `docs/00-metodo/scripts/lint_deploy.py` seguirá en rojo, y hace bien: nadie ha decidido todavía cómo se despliega esto.
