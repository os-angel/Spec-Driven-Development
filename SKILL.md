---
name: sdd-development
description: Flujo completo de Spec-Driven Development (SDD) con agentes de código. Cubre la constitución del proyecto (mission, tech-stack, roadmap), el spec de cada feature (requirements, plan, validation), la implementación por grupos de tareas, la validación con evidencia, el cierre, la replanificación y la investigación. Úsala siempre que el usuario mencione SDD, spec-driven, specs, constitución, roadmap, feature spec o siguiente fase, o pida planificar, implementar, validar o cerrar una fase, replanificar o investigar una idea para el backlog, aunque no nombre la skill. Úsala también cuando quiera definir misión, stack y roadmap de un proyecto nuevo, o introducir specs en un repositorio existente.
---

# Spec-Driven Development (SDD)

En SDD las decisiones se escriben en specs (Markdown versionado en Git) antes de escribir código. El humano decide el qué y el porqué; el agente propone, implementa el cómo y mantiene los specs al día. Como los agentes no recuerdan nada entre sesiones, los specs son la memoria del proyecto: gracias a ellos se puede limpiar el contexto sin perder el rumbo.

```
Constitución (una vez)
      |
      v
Spec de feature --> Implementación --> Validación y cierre
      ^                                        |
      |                                        v
      +---------- Replanificación <------------+
                  (entre features)
```

## Principios

Explican el resto de la skill. Cuando una situación no esté cubierta, decide con base en ellos.

1. **Spec primero.** Ningún cambio de comportamiento o de estructura entra al código sin estar antes en el spec. Si código y spec dicen cosas distintas, el proyecto pierde su memoria y se vuelve al vibe coding. Esto aplica también a las correcciones de la revisión y a los cambios que el usuario hace a mano, decantarse siempre que sea posible al enfoque everything as code.
2. **Pasos pequeños.** Una feature por rama, fases que caben en una sesión, commits frecuentes. Un diff pequeño se revisa de verdad; uno enorme se aprueba a ciegas.
3. **El nivel de detalle correcto.** Mucho contexto sobre objetivos, restricciones, decisiones y criterios de aceptación; poco sobre decisiones de bajo nivel que el agente resuelve solo, como nombres de variables o detalles internos de una función. Sobre-especificar ata al agente sin aportar nada.
4. **El humano decide, el agente propone.** Ver "Gobierno de los specs".
5. **Contexto limpio entre etapas.** Cada etapa arranca leyendo los specs, no la conversación anterior. Sugiere `/clear` antes de planificar una feature y antes de implementarla; después, el usuario vuelve a invocar la skill y el diagnóstico encuentra el estado en los archivos. Si el agente que ejecuta esta skill usa otro comando para limpiar el contexto, díselo al usuario al sugerirlo.

## Gobierno de los specs

| Tipo de cambio | Qué hace el agente |
|---|---|
| Estado: casillas, "(Completa)", enlaces entre documentos | Lo actualiza sin preguntar |
| Pedido por el usuario | Lo aplica, lo propaga a todos los documentos afectados y reporta qué cambió |
| Hallazgo del agente: regla nueva, omisión, cambio de alcance, dependencia nueva | Lo propone y espera confirmación antes de escribirlo |

La constitución es el acuerdo entre las personas del proyecto; por eso no cambia por iniciativa del agente. Los commits, merges y borrados de ramas también se proponen, y se ejecutan cuando el usuario confirma.

## Archivos

```
specs/
  mission.md              por qué existe el proyecto, para quién, alcance
  tech-stack.md           stack, entorno, pruebas, convenciones, lo que no se usa
  roadmap.md              fases pequeñas con casillas
  features/
    YYYY-MM-DD-<nombre>/
      requirements.md     alcance, fuera de alcance, decisiones, contexto
      plan.md             grupos de tareas con casillas
      validation.md       criterios verificables y definición de terminado
backlog/
  YYYY-MM-DD-<tema>.md    investigaciones no comprometidas
CHANGELOG.md
```

Cada archivo tiene su plantilla en la carpeta `assets/` de esta skill (no del proyecto): `mission.md`, `tech-stack.md`, `roadmap.md`, `requirements.md`, `plan.md`, `validation.md`, `backlog.md` y `changelog.md`. Lee la plantilla antes de crear el archivo, para que todos los proyectos compartan formato. Las plantillas están en español; escribe los specs en el idioma del proyecto. No uses emojis en los specs ni en el CHANGELOG: son documentos de ingeniería.

Las carpetas de feature llevan fecha y no número de fase porque las fases se renumeran al replanificar; la fecha no cambia.

## Diagnóstico

Haz esto cada vez que se invoque la skill, antes de elegir un modo:

1. ¿Existen `specs/mission.md`, `specs/tech-stack.md` y `specs/roadmap.md`? Revisa los archivos, no solo la carpeta.
2. ¿Hay contexto previo? `README.md`, `TODO.md`, `AGENTS.md`, `CLAUDE.md`, `SPEC.md` u otros documentos de planificación. Son insumo, pero no reemplazan la constitución.
3. `git status`: ¿hay cambios sin commitear? Si el directorio no es un repositorio Git, propón inicializarlo; el versionado es parte del método.
4. ¿En qué rama estás y cuál es la principal? Puede ser `main` o `master`: detéctala, no la asumas.
5. ¿Hay una feature a medias? Una carpeta en `specs/features/` con tareas pendientes en su `plan.md`, normalmente con su rama abierta.

| Situación | Modo |
|---|---|
| Sin constitución, repositorio vacío o casi vacío | 1. Constitución greenfield |
| Sin constitución, con código existente | 2. Constitución brownfield |
| Constitución incompleta | 1 o 2, solo para lo que falta |
| Nueva feature o "siguiente fase" | 3. Spec de feature |
| Spec aprobado con tareas pendientes, o piden implementar | 4. Implementación |
| Implementación terminada, o piden validar o cerrar | 5. Validación y cierre |
| Cambio de stack, alcance, roadmap o proceso; pedido nuevo de un stakeholder | 6. Replanificación |
| Idea para explorar sin comprometerla | 7. Investigación |
| Corrección pequeña fuera de una feature | Si cambia comportamiento, es una feature mínima: Modo 3 con un spec corto. Si no (un typo, formato), no necesita spec: se corrige y se anota en el CHANGELOG |

Antes de seguir, dile al usuario en una o dos líneas qué encontraste y qué modo vas a aplicar, para que pueda corregir el rumbo a tiempo. Si hay una feature a medias y el usuario pide otra cosa, avísale antes de cambiar de tema. Si la situación es ambigua, pregunta.

## Cómo entrevistar

Los modos 1, 2 y 3 incluyen una entrevista. Usa la herramienta de preguntas interactivas del agente si existe (en Claude Code, `AskUserQuestion`); si no, pregunta en texto. Agrupa las preguntas en bloques y, cuando el contexto lo permita, ofrece opciones con una recomendación.

No escribas ningún archivo hasta tener las respuestas. Un spec escrito antes de la entrevista refleja las suposiciones del agente, no las decisiones del usuario, y corregirlo después cuesta más que haber preguntado.

---

## Modo 1: Constitución greenfield

Objetivo: el acuerdo inicial del proyecto, antes de cualquier código.

1. Entrevista en tres bloques:
   - **Misión:** propósito, público objetivo, alcance y lo que queda fuera, cómo se ve el éxito, quién pidió qué.
   - **Stack:** lenguaje y versiones, frameworks, datos, infraestructura, entorno de desarrollo (por ejemplo, uv para Python), estrategia de pruebas, convenciones y lo que no se va a usar. Las pruebas se definen aquí porque, si se dejan para después, las primeras features quedan sin forma de validarse. Usar siempre que sea posible el enfoque everything as code (p.ej. IaC, CaC)
   - **Roadmap:** las fases conocidas, en pasos muy pequeños; cada fase debe caber en una sesión de trabajo enfocada.
2. Crea los tres archivos de la constitución a partir de las plantillas.
3. Pide revisión. Aplica los cambios cuidando que los tres documentos sigan siendo coherentes entre sí; si el usuario edita a mano, revisa que nada haya quedado desalineado.
4. Propón el commit: `docs: crea la constitución del proyecto`.

## Modo 2: Constitución brownfield

Objetivo: llevar SDD a un proyecto existente, describiendo lo que ya hay.

1. Explora antes de preguntar: archivos de dependencias (`pyproject.toml`, `requirements.txt`, `package.json`, etc.), estructura de carpetas, documentación, configuración de pruebas y CI, y `git log --oneline -20`.
2. Presenta un resumen: stack detectado con versiones, convenciones, trabajo pendiente y brechas (sin pruebas, sin CI, sin linter, etc.).
3. Entrevista sobre lo que el código no dice: misión y público objetivo, decisiones técnicas no evidentes, qué hacer con las brechas y de dónde sale el roadmap (TODO, issues, documentos).
4. Crea los tres archivos a partir de las plantillas. `tech-stack.md` describe el código real e incluye la sección de brechas; el roadmap convierte el trabajo pendiente en fases pequeñas.
5. Pide revisión y propón el commit.

## Modo 3: Spec de feature

Objetivo: decidir qué se construye y cómo se va a validar, antes de escribir código.

Antes de empezar, verifica lo siguiente. Si algo falla, díselo al usuario y pregunta cómo seguir.

- La constitución existe.
- No hay cambios sin commitear.
- La feature anterior está cerrada y mergeada.
- Se parte de la rama principal actualizada.
- La siguiente fase sigue teniendo sentido. Si el usuario duda, primero va el Modo 6.
- El contexto está limpio. Si la conversación trae trabajo previo, sugiere `/clear`.

Pasos:

1. Identifica la fase: la primera del roadmap con casillas pendientes, o la que indique el usuario.
2. Crea la rama, por ejemplo `git checkout -b feature/<nombre>`.
3. Lee `mission.md`, `tech-stack.md`, la fase en `roadmap.md` y los specs de features anteriores relacionadas.
4. Entrevista en tres bloques:
   - **Alcance:** qué hace la feature y qué queda fuera.
   - **Decisiones:** elecciones clave (datos, contratos, comportamiento, experiencia de uso) y restricciones.
   - **Validación:** cómo vamos a saber que funciona.
5. Crea `specs/features/YYYY-MM-DD-<nombre>/` con los tres archivos a partir de las plantillas:
   - `requirements.md`: el qué y el porqué.
   - `plan.md`: grupos en orden de implementación. Cada grupo deja el proyecto en un estado verificable y termina con un paso de verificación, para que se pueda revisar y commitear solo.
   - `validation.md`: criterios que el agente pueda ejecutar y comprobar por sí mismo, más los que requieren a una persona.
6. En `roadmap.md`, agrega debajo del objetivo de la fase un párrafo `**Spec:** specs/features/YYYY-MM-DD-<nombre>/`.
7. Pide revisión de los tres archivos. Al aplicar cambios, mantenlos sincronizados: un grupo nuevo en el plan suele implicar un criterio nuevo en la validación.
8. Propón el commit del spec, separado del código: `docs(spec): <nombre>`. Así queda registrado qué se acordó antes de implementar.

**Varias fases a la vez (por ejemplo, un MVP):** un solo spec con al menos un grupo por fase. Hazlo solo si la constitución y los specs anteriores son sólidos, porque cualquier hueco se multiplica. Trátalo como una prueba de los specs y, al cerrar, revisa con cuidado qué tuvo que suponer el agente.

## Modo 4: Implementación

Objetivo: convertir el plan en código, en pasos revisables.

Antes de empezar: tiene que existir un spec aprobado y commiteado para la fase, y debes estar en su rama. Si no hay spec, vuelve al Modo 3; implementar sin spec es justo lo que SDD evita. Si la conversación trae la planificación, sugiere `/clear`: el spec ya contiene lo necesario.

1. Lee `plan.md`, `requirements.md`, `validation.md` y `tech-stack.md`. Identifica los grupos pendientes por sus casillas.
2. Si el usuario no indicó el ritmo, acuérdalo: un grupo a la vez o todos los pendientes. Recomienda uno a la vez cuando hay seguridad, datos, infraestructura o algo difícil de revertir, porque ahí los errores pequeños se acumulan; todos juntos cuando el riesgo es bajo.
3. Implementa. Al terminar cada grupo, ejecuta su verificación y marca sus tareas con `[x]`. Las casillas permiten retomar en otra sesión leyendo solo el archivo.
4. Si aparece algo que el spec no cubre (una dependencia nueva, una decisión no tomada, un cambio de alcance), detente y propónlo. Las dependencias nuevas necesitan aprobación porque el stack es parte de la constitución.
5. Entrega por grupo: archivos creados o modificados, resultado de la verificación y propuesta de commit, por ejemplo `feat(<alcance>): grupo N de <nombre>`.
6. Si un grupo implicó mucha depuración, sugiere `/clear` antes del siguiente.

## Modo 5: Validación y cierre

Objetivo: comprobar que lo construido cumple el spec, que el usuario lo entiende y que todo queda registrado antes del merge.

1. **Validación técnica.** Ejecuta cada criterio de `validation.md` y reporta un scorecard con evidencia. "Funciona" sin evidencia no cuenta.

   | Criterio | Resultado | Evidencia |
   |---|---|---|
   | <criterio> | Pasa / Falla | <comando y salida resumida> |

2. **Revisión profunda**, recomendada en features grandes o sensibles. Si el agente puede lanzar sub-agentes, lanza tres que revisen los cambios de la rama desde perspectivas distintas: corrección, consistencia con los specs, y mantenibilidad o seguridad. Así el contexto principal queda limpio. Sin sub-agentes, haz tres pasadas. Presenta los hallazgos y deja que el usuario elija cuáles corregir.
3. **Revisión humana.** Sugiere qué mirar en alto nivel (¿hace lo que dice el spec?), no cada línea. Para evitar deuda cognitiva, señala los tests que mejor explican el flujo, para que el usuario los lea o los corra en el debugger.
4. **Correcciones.** Cada corrección entra primero al spec, como un grupo nuevo en `plan.md` (y en `requirements.md` o `validation.md` si cambia una decisión o un criterio), y después al código. Si el usuario cambió algo a mano, actualiza los specs y las referencias que hayan quedado desalineados.
5. **Lo que el spec no decía.** Reporta qué tuvo que decidir o suponer el agente durante la implementación y propón cómo registrarlo. Es lo que hace que el siguiente spec salga mejor.
6. **Lista de cierre.** Antes de proponer el merge, todo esto tiene que estar hecho. Son los pasos que más se olvidan: hasta el repositorio del curso de SDD dejó de cumplirlos después de las primeras fases.
   - [ ] Validación técnica completa, con evidencia
   - [ ] Todas las tareas de `plan.md` marcadas
   - [ ] Specs y código sincronizados, sin decisiones sin registrar
   - [ ] Fase marcada en `roadmap.md`: casillas en `[x]` y "(Completa)" en el título
   - [ ] `CHANGELOG.md` actualizado a partir de `git log <rama-principal>..HEAD`, redactado para alguien que no lee el código y con la fecha más reciente arriba
   - [ ] Commit final
7. **Merge.** Con confirmación del usuario: merge a la rama principal y borrado de la rama de la feature.
8. **Mirada al roadmap.** ¿La siguiente fase sigue siendo la correcta? Si hay cambios de fondo, va el Modo 6; si no, sugiere `/clear` antes de la siguiente feature.

## Modo 6: Replanificación

Objetivo: ajustar la constitución, el roadmap o el proceso entre features, de forma explícita y versionada.

1. Trabaja en una rama propia, por ejemplo `replan/<tema>`, para poder saber qué versión de la constitución produjo qué código. Las actualizaciones de estado del cierre son la excepción: van en la rama de la feature.
2. Según el tipo de cambio:
   - **Constitución (stack o misión):** actualiza el archivo y propaga el cambio a todos los specs de features, también a los ya cerrados, para que sigan describiendo el sistema real. Después ajusta el código y agrega tests si aplica.
   - **Pedido nuevo de producto:** si es pequeño, se resuelve aquí, spec primero. Si es grande, entra al roadmap como fase nueva.
   - **Roadmap:** reordena, fusiona fases que van juntas o divide las grandes, manteniéndolas pequeñas.
   - **Proceso:** si un paso se repite en cada feature, propón automatizarlo con una skill nueva o una mejora a esta. Propón; no lo crees sin confirmación.
3. Pide revisión y propón commit y merge.

## Modo 7: Investigación

Objetivo: explorar una idea sin interrumpir la feature en curso ni comprometer el roadmap.

1. No toques los specs ni el código de la rama actual.
2. Investiga con el usuario y escribe `backlog/YYYY-MM-DD-<tema>.md` a partir de la plantilla: contexto, opciones, recomendación y decisión.
3. Propón un commit solo para esa nota.
4. Si después se adopta, entra como fase en el Modo 6, con un enlace a la nota desde el roadmap. Sin ese enlace, la investigación se pierde.
