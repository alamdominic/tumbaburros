# Git & GitHub — Cheatsheet Completo

> **Control de versiones · Colaboración · Workflow moderno**  
> Guía de referencia rápida para Git y GitHub

---

## 📑 Índice

- [Configuración inicial](#configuración-inicial)
- [Crear repositorio](#crear-repositorio)
- [Estado y cambios](#estado-y-cambios)
- [Staging y Commits](#staging-y-commits)
- [Branches](#branches)
- [Merge](#merge)
- [Rebase](#rebase)
- [Remote](#remote)
- [Historial](#historial)
- [Deshacer cambios](#deshacer-cambios)
- [Stash](#stash)
- [Tags](#tags)
- [GitHub](#github)
- [.gitignore](#gitignore)
- [Alias](#alias)
- [Workflows](#workflows)
- [Troubleshooting](#troubleshooting)
- [Mejores prácticas](#mejores-prácticas)

---

## ⚙ Configuración inicial

Configura tu identidad y preferencias de Git. Estas opciones se guardan en `~/.gitconfig`.

```bash
# identidad global
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"

# editor predeterminado
git config --global core.editor "code --wait"

# ver configuración
git config --list
git config user.name
```

> **Nota:** Usa `--global` para todos los repos, o `--local` para uno específico.

---

## + Crear repositorio

```bash
# nuevo repositorio local
git init
git init nombre-proyecto

# clonar repositorio existente
git clone https://github.com/user/repo.git
git clone url carpeta-destino

# clonar branch específica
git clone -b develop url
```

> **Tip:** `git clone` ya hace `git init` automáticamente.

---

## ? Estado y cambios

```bash
# ver estado del repositorio
git status
git status -s        # formato corto

# ver cambios no staged
git diff
git diff archivo.txt

# ver cambios staged
git diff --staged
git diff --cached     # igual

# comparar branches
git diff main..develop
```

---

## ✓ Staging & Commits

### STAGING — área de preparación

```bash
# agregar archivos al stage
git add archivo.txt
git add *.js           # por patrón
git add .              # todos
git add -A             # todos + deleted

# agregar interactivo
git add -p             # por partes

# quitar del stage
git reset archivo.txt
git reset              # todos
```

### COMMITS — guardar cambios

```bash
# hacer commit
git commit -m "Mensaje descriptivo"

# commit con add automático
git commit -am "Fix bug en login"

# modificar último commit
git commit --amend
git commit --amend -m "Nuevo mensaje"

# commit vacío (útil CI/CD)
git commit --allow-empty -m "Trigger"
```

**Flujo:**  
`Working Dir` → **add** → `Staging` → **commit** → `Repository`

---

## ⎇ Branches — Ramas

### Gestión de branches

```bash
# listar branches
git branch               # locales
git branch -a            # todas
git branch -r            # remotas

# crear branch
git branch feature-login

# borrar branch
git branch -d feature-login
git branch -D feature      # forzar

# renombrar branch
git branch -m nuevo-nombre
```

### Cambiar de branch

```bash
# cambiar a branch existente
git checkout develop
git switch develop        # moderno

# crear y cambiar
git checkout -b feature-x
git switch -c feature-x

# volver a branch anterior
git checkout -
git switch -
```

> **Git 2.23+:** Usa `git switch` para branches y `git restore` para archivos.

---

## ⋈ Merge — Fusionar

```bash
# fusionar branch en la actual
git merge feature-login

# fast-forward (sin commit merge)
git merge --ff-only develop

# sin fast-forward (siempre crea commit)
git merge --no-ff feature

# abortar merge en conflicto
git merge --abort

# ver branches fusionadas
git branch --merged
git branch --no-merged
```

> **⚠ Conflictos:** Edita los archivos marcados, luego `git add` y `git commit`.

---

## ↻ Rebase

```bash
# rebase sobre otra branch
git rebase main

# rebase interactivo (reescribir)
git rebase -i HEAD~3    # últimos 3
git rebase -i abc123     # desde commit

# continuar tras resolver conflicto
git rebase --continue

# saltar commit problemático
git rebase --skip

# abortar rebase
git rebase --abort
```

> **⚠ ¡Cuidado!** Nunca rebase commits que ya están en remoto público.

---

## ☁ Remote — Remotos

### Gestionar remotos

```bash
# ver remotos
git remote
git remote -v            # con URLs

# agregar remoto
git remote add origin url

# cambiar URL
git remote set-url origin nueva-url

# eliminar remoto
git remote remove origin

# ver info del remoto
git remote show origin
```

### Sincronizar

```bash
# enviar cambios
git push origin main
git push -u origin main   # track
git push --all             # todas

# traer y fusionar
git pull origin main
git pull --rebase         # con rebase

# solo traer (sin merge)
git fetch origin
git fetch --all            # todos
```

> **Buena práctica:** `git fetch` + revisar + `git merge`

---

## 📜 Historial — log

```bash
# ver historial
git log
git log --oneline          # compacto
git log --graph            # gráfico
git log --all --graph --oneline

# últimos N commits
git log -3                 # últimos 3

# por autor
git log --author="Ana"

# por fecha
git log --since="2 weeks ago"
git log --after="2024-01-01"
```

---

## ↶ Deshacer cambios

### reset — mover HEAD

```bash
# deshacer último commit (mantener cambios)
git reset --soft HEAD~1

# deshacer commit (cambios unstaged)
git reset --mixed HEAD~1
git reset HEAD~1        # igual

# deshacer y BORRAR cambios
git reset --hard HEAD~1
git reset --hard origin/main

# quitar archivo del stage
git reset archivo.txt
```

### revert · restore

```bash
# crear commit que deshace otro
git revert abc123
git revert HEAD           # último

# descartar cambios en archivo
git restore archivo.txt
git checkout -- archivo.txt # viejo

# unstage archivo
git restore --staged archivo.txt

# restaurar todo el directorio
git restore .
```

> **⚠ reset --hard:** ¡Pérdida permanente de cambios! Úsalo con cuidado.

---

## 📦 Stash — Guardar temporal

```bash
# guardar cambios temporalmente
git stash
git stash save "WIP: login form"

# listar stashes
git stash list

# aplicar último stash
git stash apply
git stash pop              # aplica y borra

# aplicar stash específico
git stash apply stash@{2}

# borrar stash
git stash drop stash@{0}
git stash clear            # todos
```

---

## 🏷 Tags — Etiquetas

```bash
# crear tag ligero
git tag v1.0.0

# tag anotado (recomendado)
git tag -a v1.0.0 -m "Release 1.0"

# tag en commit específico
git tag -a v1.0.0 abc123

# listar tags
git tag
git tag -l "v1.*"       # filtrar

# enviar tags
git push origin v1.0.0
git push --tags            # todos

# borrar tag
git tag -d v1.0.0
git push origin --delete v1.0.0  # remoto
```

---

## ★ GitHub — Colaboración

### Fork & Clone

```bash
# 1. Fork en GitHub (botón)

# 2. Clonar tu fork
git clone tu-fork-url

# 3. Agregar upstream
git remote add upstream original-url

# 4. Sincronizar
git fetch upstream
git merge upstream/main
```

### Pull Requests

```bash
# 1. Crear branch
git checkout -b feature-x

# 2. Hacer cambios y commit
git add .
git commit -m "Add feature"

# 3. Push a tu fork
git push -u origin feature-x

# 4. Abrir PR en GitHub
```

### GitHub CLI

```bash
# instalar gh CLI
gh auth login

# crear repo
gh repo create

# crear PR
gh pr create

# ver issues
gh issue list

# crear issue
gh issue create
```

### SSH Setup

```bash
# generar clave SSH
ssh-keygen -t ed25519 -C "email"

# copiar clave pública
cat ~/.ssh/id_ed25519.pub

# agregar en GitHub:
# Settings → SSH Keys

# probar conexión
ssh -T git@github.com
```

---

## 🚫 .gitignore

Ejemplo de archivo `.gitignore`:

```gitignore
# archivos del sistema
.DS_Store
Thumbs.db

# dependencias
node_modules/
venv/
__pycache__/

# builds
dist/
build/
*.pyc

# env y secretos
.env
.env.local
*.key

# IDE
.vscode/
.idea/
*.swp
```

> **Templates:** [github.com/github/gitignore](https://github.com/github/gitignore)

---

## ⚡ Alias útiles

```bash
# configurar alias
git config --global alias.st 'status'
git config --global alias.co 'checkout'
git config --global alias.br 'branch'
git config --global alias.ci 'commit'
git config --global alias.unstage 'reset HEAD --'

# log bonito
git config --global alias.lg 'log --graph --oneline --all'

# usar alias
git st
git lg
```

---

## 🔄 Workflows comunes

### Feature Branch Workflow

```bash
# 1. Actualizar main
git checkout main
git pull origin main

# 2. Crear feature branch
git checkout -b feature/nueva-feature

# 3. Trabajar y commitear
git add .
git commit -m "Implement feature"

# 4. Push y crear PR
git push -u origin feature/nueva-feature

# 5. Tras aprobación: merge
git checkout main
git merge feature/nueva-feature
git push origin main
```

### Hotfix Workflow

```bash
# 1. Desde main crear hotfix
git checkout main
git checkout -b hotfix/critical-bug

# 2. Arreglar y commit
git commit -am "Fix critical bug"

# 3. Merge a main
git checkout main
git merge hotfix/critical-bug
git tag -a v1.0.1 -m "Hotfix"

# 4. Merge a develop también
git checkout develop
git merge hotfix/critical-bug

# 5. Push y borrar branch
git push --all --follow-tags
git branch -d hotfix/critical-bug
```

---

## 🔧 Troubleshooting — Problemas comunes

### Conflictos de merge

```bash
# 1. Git marca conflictos
<<<<<<< HEAD
tu código
=======
código incoming
>>>>>>> branch

# 2. Edita archivo
# 3. Marca como resuelto
git add archivo.txt
git commit

# abortar si falla
git merge --abort
```

### Deshacer push

```bash
# si nadie ha pulled
git reset --hard HEAD~1
git push --force

# si otros ya pulled
git revert HEAD
git push

# recuperar commit perdido
git reflog
git reset --hard abc123
```

### Limpiar repo

```bash
# borrar branches merged
git branch --merged | xargs git branch -d

# limpiar archivos ignorados
git clean -fdX

# optimizar repo
git gc
git prune

# ver tamaño
git count-objects -vH
```

---

## ✨ Mejores prácticas

### Mensajes de commit

```bash
# formato convencional
feat: Add user login
fix: Resolve null pointer
docs: Update README
style: Format code
refactor: Simplify logic
test: Add unit tests
chore: Update deps

# imperativo, presente
✓ "Add feature"
✗ "Added feature"
```

### Commits atómicos

- ✓ Un cambio lógico
- ✓ Builds sin errores
- ✓ Tests pasando
- ✓ Fácil de revertir

- ✗ Múltiples features
- ✗ Mitad de feature
- ✗ "Fix typo" x10

```bash
# squash si es necesario
git rebase -i HEAD~5
```

### Branches

```bash
# nomenclatura clara
feature/user-auth
bugfix/login-error
hotfix/critical-bug
release/v1.2.0

# proteger main/develop
# branch protection rules
# require PR reviews
# status checks
```

### Colaboración

**✓ Hacer:**

- Pull antes de push
- Comunicar cambios
- Code reviews
- CI/CD integrado
- README actualizado

**✗ Evitar:**

- Force push a main
- Commits con secretos
- Branches huérfanas

---

## 📚 Recursos adicionales

- **Documentación oficial:** [git-scm.com](https://git-scm.com)
- **GitHub Docs:** [docs.github.com](https://docs.github.com)
- **Git Flow:** [nvie.com/posts/a-successful-git-branching-model](https://nvie.com/posts/a-successful-git-branching-model/)
- **Conventional Commits:** [conventionalcommits.org](https://www.conventionalcommits.org/)
- **Git Cheat Sheet oficial:** [training.github.com](https://training.github.com/)

---

**Hecho con ❤️ por Alam Dominic**  
_Data Engineering | Full Stack Developer_

Control de versiones profesional con Git & GitHub
