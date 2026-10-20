# Seguridad

## Reportar un problema

Si encuentras una vulnerabilidad o un secreto expuesto en un repo de `latam-gpt`, **no abras un issue**. Avisa directamente a un owner de la organización.

## Si subiste un secreto por error

El orden importa: borrar el archivo no alcanza, porque el secreto queda en el historial y en los clones.

1. **Rótalo ahora**: revoca el token y crea uno nuevo en el proveedor (Hugging Face, W&B, OpenAI, la nube que corresponda).
2. Avisa al lead de tu área y a un owner.
3. Saca el archivo con un PR y agrega la ruta a `.gitignore`.
4. Revisa en el proveedor si el token se usó mientras estuvo expuesto.
5. Si hace falta limpiar el historial, lo hace un owner, porque obliga a reescribir ramas de todos.

## Cómo se previene

- `.env` está en el `.gitignore` de la plantilla; solo se versiona `.env.example`.
- pre-commit corre gitleaks y bloquea llaves privadas y archivos grandes.
- La CI corre gitleaks sobre todo el historial.
- El hook del plugin `latamgpt` de Claude Code bloquea commits con `.env` o secretos.
