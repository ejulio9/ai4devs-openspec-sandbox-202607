# Entrega

## 1. Evidencia de OpenSpec

Versión instalada:

```console
➜  ai4devs-openspec-sandbox-202607 git:(main) ✗ openspec --version
1.6.0
```

Estructura del directorio `openspec`:

```console
➜  ai4devs-openspec-sandbox-202607 git:(main) ✗ ls -R openspec
changes     config.yaml specs

openspec/changes:
archive

openspec/changes/archive:
```

## 2. Plantilla de los 3 pilares

### Micro-tarea

> Generador de slugs a partir de un título (ej. "Cómo aprender Node.js en 30 días!" → `como-aprender-node-js-en-30-dias`).

### Pilar 1 — Herramienta

- **¿Cuál eliges?** Claude Code, integrado como extensión en VS Code.
- **¿Por qué esta y no otra?** Porque trabaja directamente sobre el workspace: crea el archivo, lo edita y puede ejecutar los tests en la misma sesión, sin copiar y pegar código desde un chat web. Para una micro-tarea autocontenida como esta, un asistente en el editor cierra el ciclo generar → probar → corregir mucho más rápido.

### Pilar 2 — Contexto

- **¿Qué información estás aportando?** Lenguaje (JavaScript/Node, sin dependencias externas), los requisitos del slug (minúsculas, sin acentos ni "ñ", caracteres no alfanuméricos convertidos a guion, sin guiones duplicados ni al inicio/final) y un ejemplo de entrada/salida esperada para eliminar ambigüedad.
- **¿Hay algo del contexto que has decidido omitir conscientemente?** Sí: no menciono librerías existentes (como `slugify` de npm) porque quiero una implementación propia sin dependencias, y no especifico el manejo de alfabetos no latinos (cirílico, chino…) porque queda fuera del alcance de la micro-tarea.

### Pilar 3 — Prompt

- **¿Cómo lo estructuras?** Instrucción directa + requisitos en lista + formato de salida explícito (función + casos de prueba). El ejemplo de entrada/salida actúa como mini *few-shot* y como criterio de aceptación verificable.
- **Prompt final:**

```text
Escribe en JavaScript (Node, sin dependencias) una función slugify(titulo)
que convierta un título en un slug para URL. Requisitos:
- minúsculas
- sin acentos ni "ñ" (normalizar a ASCII)
- todo carácter no alfanumérico se convierte en guion
- sin guiones duplicados ni al inicio/final
Ejemplo: "Cómo aprender Node.js en 30 días!" -> "como-aprender-node-js-en-30-dias"
Incluye 5 casos de prueba con console.assert.
```

### Resultado

- **¿Funcionó a la primera o tuviste que iterar?** Funcionó casi a la primera: la lógica general fue correcta desde el inicio, pero hubo que hacer una iteración para ajustar la expresión regular que elimina los diacríticos tras `normalize('NFD')`.
- **Una mejora que harías si volvieras a hacerlo:** Incluir en el propio prompt los casos límite como tests obligatorios (ñ, espacios en los extremos, guiones repetidos, símbolos como % o &), de modo que la primera respuesta ya venga validada contra ellos.

## 3 - Observaciones de la exploración.

Venia trabajando con algo similar, en donde primero creaba documentos como: una idea, vision, arquitectura, features y task. Con estos documentos más mi validación comenzaba a construir con Claude.

Me parece que con openspec es más simple, en menos MD podría obtener mejor o mismo resultado.