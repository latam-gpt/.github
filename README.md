# .github de latam-gpt

Este repo trae los archivos que GitHub usa por defecto en todos los repos de la organización que no tengan los suyos:

| Archivo | Para qué |
|---|---|
| `CONTRIBUTING.md` | Cómo trabajamos: repos, ramas, commits, PRs, secretos, datos y Python |
| `SECURITY.md` | Qué hacer si aparece un secreto o una vulnerabilidad |
| `PULL_REQUEST_TEMPLATE.md` | Plantilla de PR |
| `ISSUE_TEMPLATE/` | Formularios de issue: error, tarea y experimento |
| `workflow-templates/` | CI de Python lista para agregar desde la pestaña Actions |

GitHub solo aplica estos archivos si este repo es **público**, así que aquí no va nada interno. El mapa de la organización, los teams y los procesos internos viven en `.github-private` y `organizacion`, que son privados.

Para cambiar una convención, abre un PR aquí y otro en `latam-gpt/claude-plugins` (skill `convenciones`), para que las dos fuentes no se separen.
