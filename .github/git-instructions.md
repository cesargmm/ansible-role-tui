# Instrucciones de Git - Conventional Commits

## ¿Qué son los Conventional Commits?

Conventional Commits es una especificación para dar significado a los mensajes de commits, haciéndolos legibles tanto para humanos como para máquinas. Esta convención se alinea con [Semantic Versioning (SemVer)](https://semver.org/) y facilita la generación automática de CHANGELOGs.

## Estructura de un Conventional Commit

```
<tipo>: <descripción> [issue#X]

[cuerpo opcional]

[nota(s) al pie opcional(es)]
```

**Nota**: El `[issue#X]` se añade automáticamente al final del mensaje basándose en el nombre de la rama.

### Tipos de Commits

- **feat**: Una nueva característica para el usuario
- **fix**: Corrección de un bug
- **docs**: Cambios en la documentación
- **style**: Cambios que no afectan el significado del código (espacios en blanco, formato, etc.)
- **refactor**: Cambio de código que no corrige un bug ni añade una característica
- **perf**: Cambio de código que mejora el rendimiento
- **test**: Añadir tests faltantes o corregir tests existentes
- **build**: Cambios que afectan el sistema de build o dependencias externas
- **ci**: Cambios en archivos de configuración y scripts de CI
- **chore**: Otros cambios que no modifican archivos src o test
- **revert**: Revierte un commit anterior

### Ejemplos

```bash
# Commit simple (el hook añadirá automáticamente [issue#4])
git commit -m "feat: add support for multiple network interfaces"
# Resultado: "feat: add support for multiple network interfaces [issue#4]"

# Commit de corrección
git commit -m "fix: correct IP address detection for non-eth0 interfaces"
# Resultado: "fix: correct IP address detection for non-eth0 interfaces [issue#4]"

# Commit con cuerpo
git commit -m "feat: add docker network support

Add functionality to detect and display all active network interfaces
including docker and bridge interfaces"
# Resultado: "feat: add docker network support [issue#4]" + cuerpo

# Breaking change
git commit -m "feat!: change network interface detection method

BREAKING CHANGE: The old eth0-only detection is replaced with dynamic
interface discovery. Update your configuration if you relied on the
specific eth0 output format."
# Resultado: "feat!: change network interface detection method [issue#4]" + cuerpo
```

## REGEX para Conventional Commits

### REGEX Básico (sin ámbito, con issue)

```regex
^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)(!)?:\s.{1,50}\s\[issue#\d+\]
```

### REGEX Completo (con breaking changes y múltiples líneas)

```regex
^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)(!)?:\s.{1,72}\s\[issue#\d+\](\n\n.*)?(\n\n(BREAKING CHANGE:\s.+|[\w-]+:\s.+))?$
```

### Componentes del REGEX

- `^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)`: Tipo de commit (requerido)
- `(!)?`: Indicador opcional de breaking change
- `:\s`: Dos puntos seguido de un espacio (requerido)
- `.{1,72}`: Descripción de 1 a 72 caracteres
- `\s\[issue#\d+\]`: Referencia al issue entre corchetes (requerido)
- `(\n\n.*)?`: Cuerpo opcional (separado por dos saltos de línea)
- `(\n\n(BREAKING CHANGE:\s.+|[\w-]+:\s.+))?`: Footer opcional

## Validación con Git Hooks

### Usando commitlint (Recomendado)

```bash
# Instalar commitlint
npm install --save-dev @commitlint/cli @commitlint/config-conventional

# Crear archivo de configuración
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js

# Instalar husky para git hooks
npm install --save-dev husky
npx husky install
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit $1'
```

### Script de validación con auto-adición de issue

```bash
#!/bin/bash
# .git/hooks/commit-msg

commit_msg_file="$1"
commit_msg=$(cat "$commit_msg_file")

# Extraer el número de issue de la rama actual
branch_name=$(git symbolic-ref --short HEAD 2>/dev/null)
issue_number=$(echo "$branch_name" | grep -oP 'issue#\K\d+')

# Si hay un número de issue y el mensaje no lo tiene ya
if [ -n "$issue_number" ] && ! echo "$commit_msg" | grep -qE "\[issue#\d+\]"; then
    # Obtener la primera línea del commit
    first_line=$(echo "$commit_msg" | head -n1)
    rest_of_msg=$(echo "$commit_msg" | tail -n +2)
    
    # Añadir el issue al final de la primera línea
    new_first_line="${first_line} [issue#${issue_number}]"
    
    # Reconstruir el mensaje
    if [ -n "$rest_of_msg" ]; then
        echo -e "${new_first_line}${rest_of_msg}" > "$commit_msg_file"
    else
        echo "${new_first_line}" > "$commit_msg_file"
    fi
    
    # Actualizar la variable para la validación
    commit_msg=$(cat "$commit_msg_file")
fi

# Validar el formato
pattern="^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)(!)?:\s.{1,72}\s\[issue#\d+\]"

if ! echo "$commit_msg" | grep -qE "$pattern"; then
    echo "❌ Error: El mensaje del commit no sigue Conventional Commits"
    echo ""
    echo "Formato esperado:"
    echo "  <tipo>: <descripción> [issue#X]"
    echo ""
    echo "Tipos válidos: feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert"
    echo ""
    echo "Ejemplo: feat: add multiple interface support [issue#4]"
    echo ""
    echo "Nota: El [issue#X] se añade automáticamente si tu rama sigue el formato 'feature/issue#X-descripcion'"
    exit 1
fi
```

### Instalación del hook

```bash
# Copiar el script al directorio de hooks
cp .github/commit-msg .git/hooks/commit-msg

# Dar permisos de ejecución
chmod +x .git/hooks/commit-msg
```

## Herramientas Útiles

- **[commitizen](https://github.com/commitizen/cz-cli)**: CLI interactivo para crear commits
- **[commitlint](https://commitlint.js.org/)**: Linter para mensajes de commit
- **[standard-version](https://github.com/conventional-changelog/standard-version)**: Generación automática de CHANGELOG y versionado
- **[semantic-release](https://github.com/semantic-release/semantic-release)**: Automatización completa de releases

## Referencias

- [Conventional Commits Specification](https://www.conventionalcommits.org/)
- [Angular Commit Guidelines](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#commit)
