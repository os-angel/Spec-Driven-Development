<div align="center">

# Spec-Driven Development

**Una skill para trabajar con agentes de código donde las decisiones se escriben antes que el código.**


![Idioma](https://img.shields.io/badge/idioma-español-2F6FEB?style=flat-square)
![Specs](https://img.shields.io/badge/specs-Markdown_+_Git-555555?style=flat-square)

</div>

---

Los agentes no recuerdan nada entre sesiones. En SDD los specs son la memoria del proyecto: el humano decide el qué y el porqué, el agente propone, implementa el cómo y mantiene los specs al día.

```
Constitución (una vez)
      |
      v
Spec de feature --> Implementación --> Validación y cierre
      ^                                        |
      |                                        v
      +---------- Replanificación <------------+
```

## Instalación

Copia esta carpeta en el directorio de skills de Claude Code:

```bash
# Para todos tus proyectos
git clone https://github.com/os-angel/Spec-Driven-Development.git ~/.claude/skills/sdd-development

# Solo para un proyecto
git clone https://github.com/os-angel/Spec-Driven-Development.git .claude/skills/sdd-development
```

## Uso

No hace falta nombrarla. Se activa cuando hablas de specs, roadmap o fases:

> "Define la constitución de este proyecto"
> "Planifica la siguiente fase"
> "Valida y cierra la feature"

En cada invocación la skill revisa el estado del repositorio y elige el modo:

| Modo | Para qué |
|---|---|
| Constitución | Misión, stack y roadmap, en proyectos nuevos o existentes |
| Spec de feature | Requisitos, plan y criterios de validación |
| Implementación | Grupos de tareas pequeños, con commits frecuentes |
| Validación y cierre | Evidencia, CHANGELOG y estado del roadmap |
| Replanificación | Cambios de alcance, stack o prioridades |
| Investigación | Ideas exploradas sin comprometerlas |

## Lo que genera en tu proyecto

```
specs/
  mission.md
  tech-stack.md
  roadmap.md
  features/
    YYYY-MM-DD-<nombre>/
      requirements.md
      plan.md
      validation.md
backlog/
  YYYY-MM-DD-<tema>.md
CHANGELOG.md
```

## Contenido del repositorio

- `SKILL.md`: las instrucciones de la skill
- `assets/`: plantillas de cada documento, para que todos los proyectos compartan formato
