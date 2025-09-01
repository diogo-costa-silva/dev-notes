# 📚 Dev Wiki Setup Guide

Este documento contém todos os comandos e passos necessários para criar e manter uma dev wiki similar usando MkDocs Material.

## 🚀 Setup Inicial

### 1. Criar Projeto
```bash
# Criar diretório do projeto
mkdir my-dev-wiki && cd my-dev-wiki

# Inicializar Git
git init
git branch -M main
```

### 2. Setup Python Environment
```bash
# Criar projeto Python com UV
uv init

# Adicionar dependências do MkDocs
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

# Criar estrutura de diretórios
mkdir -p docs/templates/{setup,languages,tools,data,assets}
```

## ⚙️ Configuração

### 4. Configurar mkdocs.yml
Copiar o ficheiro `mkdocs.yml` deste projeto ou usar como base a seguinte configuração:

```yaml
site_name: Minha Dev Wiki
site_description: Notas de programação e templates
site_url: https://username.github.io/dev-wiki/

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
  - git-revision-date-localized:
      enable_creation_date: true
      type: timeago
  - minify:
      minify_html: true
  - awesome-pages
  - section-index

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

repo_url: https://github.com/username/dev-wiki
edit_uri: ""

extra:
  social:
    - icon: fontawesome/brands/github
      link: https://github.com/username
```

### 5. Configurar Navegação Automática
Criar ficheiros `.pages` em cada diretório para controlar a navegação:

```bash
# Raiz dos docs
echo "nav:
  - index.md
  - templates" > docs/.pages

# Templates principais  
echo "title: Templates & Guias
nav:
  - index.md
  - setup
  - languages
  - tools
  - data
  - assets" > docs/templates/.pages

# Para cada subcategoria
echo "title: Configuração Inicial
nav:
  - index.md
  - ..." > docs/templates/setup/.pages
```

## 🌐 Deploy & GitHub Pages

### 6. Configurar GitHub Repository
```bash
# Criar repositório no GitHub (via CLI)
gh repo create dev-wiki --public --source=. --push

# Ou adicionar remote manualmente
git remote add origin https://github.com/username/dev-wiki.git
```

### 7. Setup GitHub Actions
Criar `.github/workflows/deploy.yml`:

```yaml
name: Deploy MkDocs

on:
  push:
    branches: [ main ]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-python@v5
      with:
        python-version: 3.x
    - run: pip install mkdocs-material mkdocs-awesome-pages-plugin mkdocs-section-index mkdocs-git-revision-date-localized-plugin mkdocs-minify-plugin pymdown-extensions
    - run: mkdocs build
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
    - uses: actions/deploy-pages@v4
      id: deployment
```

## 💻 Comandos de Desenvolvimento

### Servidor Local
```bash
# Iniciar servidor de desenvolvimento
uv run mkdocs serve

# Servidor com auto-reload
uv run mkdocs serve --dev-addr=0.0.0.0:8000
```

### Build & Deploy
```bash
# Build local
uv run mkdocs build

# Deploy manual para GitHub Pages
uv run mkdocs gh-deploy

# Limpar cache
uv run mkdocs build --clean
```

### Manutenção
```bash
# Atualizar dependências
uv sync --upgrade

# Verificar configuração
uv run mkdocs config

# Validar links
uv run mkdocs serve --strict
```

## 📁 Estrutura Recomendada

```
my-dev-wiki/
├── docs/
│   ├── index.md                 # Página principal
│   ├── .pages                   # Configuração navegação raiz
│   └── templates/
│       ├── index.md            # Overview templates
│       ├── .pages              # Configuração navegação
│       ├── setup/              # Configurações iniciais
│       │   ├── index.md
│       │   ├── .pages
│       │   └── ...
│       ├── languages/          # Linguagens programação
│       │   ├── index.md
│       │   ├── .pages
│       │   ├── python/
│       │   └── node/
│       ├── tools/              # Ferramentas dev
│       │   ├── index.md
│       │   ├── .pages
│       │   ├── git/
│       │   └── ...
│       ├── data/               # Data analysis
│       │   ├── index.md
│       │   ├── .pages
│       │   └── ...
│       └── assets/             # Recursos partilhados
│           └── images/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── mkdocs.yml
├── pyproject.toml
└── README.md
```

## 🎯 Melhores Práticas

### Organização de Conteúdo
- **Um `index.md` por pasta** para melhor SEO e navegação
- **Categorias lógicas** (setup, languages, tools, data)
- **Nomenclatura consistente** em português ou inglês
- **Links relativos** sempre que possível

### Performance
- **Otimizar imagens** antes de adicionar aos assets
- **Usar minify plugin** para produção  
- **Lazy loading** para conteúdo pesado

### Manutenção
- **Datas de revisão automáticas** com git-revision-date plugin
- **Validação de links** regular
- **Backup regular** do conteúdo importante

---

> 💡 **Dica:** Este setup está otimizado para uma wiki pessoal de desenvolvimento. Adapta conforme as tuas necessidades específicas!