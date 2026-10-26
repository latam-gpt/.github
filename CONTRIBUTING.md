# Cómo trabajamos en LatamGPT

Estas reglas valen para todos los repos de `latam-gpt` que no tengan su propio CONTRIBUTING. Lo que se puede automatizar ya está en la plantilla de repo, en pre-commit, en la CI y en el plugin de Claude Code `latamgpt`.

## Antes de empezar

- Cuenta de GitHub con 2FA activo.
- En cada repo que clones: `uvx pre-commit install`.
- Si usas Claude Code, al abrir un repo te va a ofrecer instalar el plugin `latamgpt`: acéptalo.

## Repos

- Nombre en minúsculas con guiones (`dialect-judge`, no `Dialect_Judge`).
- Descripción obligatoria, de una línea.
- Dos topics por repo: uno de área y uno de estado.

| Topic de área | Qué va ahí |
|---|---|
| `area-datos` | Corpus, scraping, filtros y clasificadores de calidad |
| `area-sintetico` | Generación de datos sintéticos |
| `area-entrenamiento` | Pretraining, post-training y ambientes de RL |
| `area-evaluacion` | Benchmarks y evaluación de modelos |
| `area-producto` | Copuchat y aplicaciones |
| `area-infra` | Clústeres, servicio de modelos e infraestructura |
| `area-investigacion` | Papers, reportes y exploraciones |

| Topic de estado | Significa |
|---|---|
| `estado-activo` | Se usa y se mantiene |
| `estado-experimento` | Exploración con fecha de término en el README |
| `estado-deprecado` | Reemplazado; el README dice por cuál |

- Los repos nuevos se crean desde la plantilla `latam-gpt/template-python`.
- El README dice qué hace el repo, su estado, quién es el dueño, cómo correrlo y dónde quedan sus datos y modelos.
- Un experimento dentro de un repo va en `experiments/<AAAA-MM-DD>-<slug>/` con su README: hipótesis, config, semilla, comando, corrida de W&B, resultados, conclusión y fecha de término. Al terminar, se promueve a código mantenido o se borra la carpeta.
- Antes de crear un repo nuevo, revisa si su lugar es una carpeta en un repo que ya existe.

## Ramas y commits

- `main` es la rama por defecto y siempre funciona.
- Se trabaja en ramas cortas: `feat/`, `fix/`, `exp/`, `docs/` o `chore/` más una descripción breve (`feat/tokenizacion-qwen35`).
- Los mensajes siguen [Conventional Commits](https://www.conventionalcommits.org/es/v1.0.0/): `tipo(alcance opcional): descripción`. Tipos: `feat`, `fix`, `docs`, `refactor`, `test`, `perf`, `ci`, `chore`.
- Sin coautoría de IA: no agregues `Co-Authored-By: Claude` ni "Generated with Claude Code" en commits ni PRs.

## Pull requests

- Lo recomendado es que todo cambio a `main` entre por PR, para que corra la CI.
- No hace falta aprobación: quien hizo el cambio lo mergea. El lead del área se preocupa de que el repo esté ordenado ([decisión 0006](https://github.com/latam-gpt/organizacion/blob/main/decisiones/0006-todos-mergean.md)).
- El título del PR sigue Conventional Commits y se mergea con squash, con la CI en verde.
- El plugin `latamgpt` de Claude Code arma la rama, el commit y el PR con este formato.

## Secretos

- Nunca van en git. Usa un `.env` local (ignorado) y versiona solo `.env.example` con los nombres de las variables.
- Los tokens se leen de variables de entorno, nunca se escriben en el código ni en notebooks.
- Si subiste un secreto por error, sigue [SECURITY.md](SECURITY.md): primero se rota y después se saca.

## Datos, modelos y archivos grandes

- Nada de más de 5 MB en git; pre-commit lo bloquea.
- Datos y modelos van al Hub de Hugging Face, en la organización `latam-gpt`, privados por defecto y con su dataset card o model card.
- Antes de subir datos, revisa que no tengan datos personales.
- Los notebooks se versionan sin salidas (nbstripout lo hace solo).

## Python

- uv, `pyproject.toml` y `uv.lock`, con la versión de Python fijada en `.python-version`.
- ruff para lint y formato, pytest para tests.
- Las dependencias quedan fijadas en el lock. Si una librería necesita un parche, se hace un fork en `latam-gpt/<librería>` fijado por commit; nunca se edita `site-packages`.

## Código y métodos de otros

- Si un repo o una carpeta es fork o copia de otro repo, o implementa un paper, el README lo dice al inicio con el enlace a la fuente original y su licencia.
- Al fusionar o archivar un repo, esos enlaces pasan al README del destino. La fuente original nunca se pierde.
- Los orígenes conocidos están registrados en el campo `origen` de `repos.yaml`, en el repo `organizacion`.

## Corridas y evaluación

- W&B: un proyecto por repo; corridas nombradas `<modelo>-<etapa>-<fecha>`.
- Todo checkpoint que se quiera comparar se evalúa con `latamgpt-eval`, en safetensors de Hugging Face.

## Claude Code

- El plugin `latamgpt` trae estas convenciones como skills (`convenciones`, `nuevo-repo`, `experimento`, `publicar-hf`, `evaluar-checkpoint`, `claude-md`) y un hook que bloquea commits con secretos, archivos grandes o coautoría de IA, y `push --force` a `main`.
- Cada repo tiene un `CLAUDE.md` con lo propio: comandos exactos, estructura y trampas conocidas. Lo común vive en el plugin.
