# 🛠️ Dev Wiki Setup Guide

Guia completo para reproduzir este projeto de wiki de desenvolvimento usando MkDocs Material + UV.

## 📋 **Sobre este Setup**

Este projeto usa:
- **[MkDocs Material](https://squidfunk.github.io/mkdocs-material/)** - Gerador de sites estáticos moderno
- **[UV Package Manager](https://docs.astral.sh/uv/)** - Gestor de pacotes Python ultra-rápido
- **GitHub Actions** - Deploy automático para GitHub Pages
- **Awesome Pages Plugin** - Navegação avançada automática

---

## 🚀 **Reprodução Rápida**

### Pré-requisitos
```bash
# Instalar UV (se não tiver)
curl -LsSf https://astral.sh/uv/install.sh | sh

# Verificar instalação
uv --version
```

### Setup Completo
```bash
# 1. Clone/fork este repositório
git clone https://github.com/diogo-costa-silva/dev-notes.git
cd dev-notes

# 2. Instalar dependências
uv sync

# 3. Testar localmente
uv run mkdocs serve

# 4. Aceder a http://localhost:8000
```

---

## 📦 **Criação desde o Zero**

### 1. Inicializar Projeto

```bash
# Criar diretório e navegar
mkdir minha-dev-wiki && cd minha-dev-wiki

# Inicializar Git
git init
git branch -M main

# Inicializar projeto Python com UV
uv init --name "dev-wiki" --python "3.11"
```

### 2. Adicionar Dependências MkDocs

```bash
# Adicionar todas as dependências necessárias
uv add mkdocs-material \
       mkdocs-awesome-pages-plugin \
       mkdocs-section-index \
       mkdocs-git-revision-date-localized-plugin \
       mkdocs-minify-plugin \
       mkdocs-redirects \
       pymdown-extensions
```

### 3. Inicializar MkDocs

```bash
# Criar estrutura básica do MkDocs
uv run mkdocs new .

# Criar estrutura de diretórios organizada
mkdir -p docs/{setup/{macos,ai_tools},languages/{python,node},tools/{git,claude_code,ai_tools},data_analysis/{methodology,modeling,sql},assets/images}
```

### 4. Configuração Principal

Criar ficheiro `mkdocs.yml`:

```yaml
site_name: Minha Dev Wiki
site_description: Notas de programação e templates
site_url: https://username.github.io/repo-name/

theme:
  name: material
  language: pt
  features:
    - navigation.tabs
    - navigation.tabs.sticky
    - navigation.sections
    - navigation.expand
    - navigation.top
    - navigation.tracking
    - content.code.copy
    - content.code.select
    - content.code.annotate
    - search.suggest
    - search.highlight
    - search.share
    - toc.integrate
  palette:
    # Light mode
    - media: "(prefers-color-scheme: light)"
      scheme: default
      primary: blue grey
      accent: indigo
      toggle:
        icon: material/brightness-7
        name: Switch to dark mode
    # Dark mode
    - media: "(prefers-color-scheme: dark)"
      scheme: slate
      primary: blue grey
      accent: indigo
      toggle:
        icon: material/brightness-4
        name: Switch to light mode

plugins:
  - search:
      lang: pt
  - awesome-pages
  - section-index
  - git-revision-date-localized:
      enable_creation_date: true
      type: timeago
      locale: pt
  - minify:
      minify_html: true
      minify_js: true
      minify_css: true
      htmlmin_opts:
        remove_comments: true

markdown_extensions:
  - admonition
  - toc:
      permalink: true
      title: Nesta página
  - pymdownx.details
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
  - pymdownx.highlight:
      anchor_linenums: true
      line_spans: __span
      pygments_lang_class: true
  - pymdownx.inlinehilite
  - pymdownx.snippets
  - pymdownx.tabbed:
      alternate_style: true
  - pymdownx.tasklist:
      custom_checkbox: true
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
  - attr_list
  - md_in_html

repo_url: https://github.com/username/repo-name
edit_uri: ""

extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/username

nav:
  - Home: index.md
  - Setup: setup/index.md
  - Languages: languages/index.md
  - Tools: tools/index.md
  - Data Analysis: data_analysis/index.md
```

---

## 🗂️ **Estrutura de Navegação (.pages)**

### Ficheiros .pages para navegação automática:

#### `docs/.pages`
```yaml
nav:
  - index.md
  - setup
  - languages  
  - tools
  - data_analysis
```

#### `docs/setup/.pages`
```yaml
title: Setup & Configuration
nav:
  - index.md
  - macos
  - ai_tools
```

#### `docs/tools/.pages`
```yaml
title: Development Tools
nav:
  - index.md
  - git
  - claude_code
  - ai_tools
```

#### `docs/data_analysis/.pages`
```yaml
title: Data Analysis & Modeling
nav:
  - index.md
  - methodology
  - modeling
  - sql
```

---

## 🌐 **GitHub Actions Deploy**

### Configurar Repository GitHub
```bash
# Criar repositório (via GitHub CLI)
gh repo create minha-dev-wiki --public --source=. --push

# Ou adicionar remote manualmente
git remote add origin https://github.com/username/minha-dev-wiki.git
```

### Workflow File: `.github/workflows/gh-pages.yml`

```yaml
name: Deploy MkDocs Website to GitHub Pages

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: write
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0
      - uses: astral-sh/setup-uv@v3
        with:
          python-version: "3.12"
      - run: uv sync
      - run: uv run mkdocs build --strict
      - uses: actions/upload-pages-artifact@v3
        with:
          path: site

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

---

## 🔧 **Scripts de Desenvolvimento**

### Comandos Essenciais

```bash
# Desenvolvimento local
uv run mkdocs serve                    # Servidor com hot-reload
uv run mkdocs serve --dev-addr=0.0.0.0:8000  # Acessível na rede

# Build e validação
uv run mkdocs build                    # Build estático
uv run mkdocs build --clean            # Build limpo
uv run mkdocs build --strict           # Build com validação rigorosa

# Manutenção
uv sync --upgrade                      # Atualizar dependências
```

### Scripts Úteis (package.json equivalente)

Adicionar ao `pyproject.toml`:

```toml
[project.scripts]
serve = "mkdocs serve"
build = "mkdocs build"
deploy = "mkdocs gh-deploy"
```

---

## 📁 **Estrutura de Ficheiros**

```
minha-dev-wiki/
├── docs/                              # Conteúdo da wiki
│   ├── index.md                       # Página principal
│   ├── .pages                         # Navegação raiz
│   ├── setup/                         # Configuração inicial
│   │   ├── index.md
│   │   ├── .pages
│   │   ├── macos/                     # Setup macOS
│   │   │   ├── index.md
│   │   │   ├── .pages
│   │   │   └── homebrew/
│   │   └── ai_tools/                  # Ferramentas AI
│   │       ├── index.md
│   │       ├── .pages
│   │       └── ollama.md
│   ├── languages/                     # Linguagens programação
│   │   ├── index.md
│   │   ├── .pages
│   │   ├── python/
│   │   └── node/
│   ├── tools/                         # Ferramentas dev
│   │   ├── index.md
│   │   ├── .pages
│   │   ├── git/
│   │   ├── claude_code/
│   │   └── ai_tools/
│   ├── data_analysis/                 # Análise dados
│   │   ├── index.md
│   │   ├── .pages
│   │   ├── methodology/
│   │   ├── modeling/
│   │   └── sql/
│   └── assets/                        # Recursos partilhados
│       └── images/
├── .github/workflows/                 # GitHub Actions
│   └── gh-pages.yml
├── .gitignore                         # Git exclusions
├── mkdocs.yml                         # Configuração MkDocs
├── pyproject.toml                     # Dependências Python
├── README.md                          # Documentação projeto
└── DEV_WIKI_SETUP.md                 # Este ficheiro
```

---

## 🎯 **Best Practices**

### Organização de Conteúdo
- ✅ **Index em cada pasta** para melhor SEO e navegação
- ✅ **Categorias lógicas** (setup → languages → tools → data)
- ✅ **Nomenclatura consistente** sem espaços nos nomes de ficheiros
- ✅ **Links relativos** sempre que possível

### Performance & SEO
- ✅ **Minify plugin** ativo para produção
- ✅ **Imagens otimizadas** antes de adicionar aos assets
- ✅ **Meta descriptions** em cada página principal
- ✅ **Proper heading structure** (H1 → H2 → H3)

### Manutenção
- ✅ **Git revision dates** automáticas
- ✅ **Strict build** para detetar erros
- ✅ **Link validation** regular
- ✅ **Dependency updates** com `uv sync --upgrade`

---

## 🐛 **Troubleshooting**

### Problemas Comuns

#### Plugin não encontrado
```bash
# Verificar se plugins estão instalados
uv run python -m pip list | grep mkdocs
```

#### Build falha com links quebrados
```bash
# Build com validação para encontrar problemas
uv run mkdocs build --strict
```

#### Pages não aparecem na navegação
```bash
# Verificar ficheiros .pages e nav no mkdocs.yml
# Awesome pages plugin precisa de estar ativo
```

#### GitHub Actions falha
```bash
# Verificar:
# 1. Permissões do repositório (Settings → Pages → Source: GitHub Actions)  
# 2. Workflow file está correto
# 3. Dependencies no pyproject.toml estão completas
```

---

## 🚀 **Deploy & Go Live**

### Primeiro Deploy
```bash
# 1. Commit todas as mudanças
git add .
git commit -m "Initial dev wiki setup"

# 2. Push para GitHub
git push origin main

# 3. Ativar GitHub Pages
# GitHub Repository → Settings → Pages → Source: GitHub Actions

# 4. Site live em ~2 minutos em:
# https://username.github.io/repo-name/
```

### Atualizações
```bash
# Qualquer push para main → deploy automático
git add .
git commit -m "Update content"
git push origin main
```

---

## 📚 **Recursos Adicionais**

- **[MkDocs Material](https://squidfunk.github.io/mkdocs-material/)** - Documentação oficial
- **[UV Documentation](https://docs.astral.sh/uv/)** - Guia completo do UV
- **[Awesome Pages Plugin](https://github.com/lukasgeiter/mkdocs-awesome-pages-plugin)** - Navegação avançada
- **[GitHub Actions](https://docs.github.com/en/actions)** - CI/CD documentation

---

> 💡 **Dica Final:** Começaste com este template, agora personaliza o conteúdo conforme as tuas necessidades! Este setup está otimizado para produtividade e manutenção a longo prazo.