# ⚡ UV Complete Guide - Modern Python Development

O UV é o gestor de pacotes Python ultra-rápido que vai revolucionar o teu workflow. **10-100x mais rápido** que pip/conda!

## 🚀 **Quick Start**

### Installation
```bash
# macOS/Linux - Método recomendado
curl -LsSf https://astral.sh/uv/install.sh | sh

# Homebrew (alternativa)
brew install uv

# Verificar instalação
uv --version
```

### Cloning & Setup (Para novos utilizadores)
```bash
# 1. Clone do projeto existente
git clone <repository-url>
cd <project-name>

# 2. Setup automático com UV
uv sync                         # Instala dependências do pyproject.toml + uv.lock
uv run python main.py          # Executa sem ativar ambiente

# 🔄 Método tradicional (se necessário)
python -m venv .venv
source .venv/bin/activate       
pip install -r requirements.txt
```

> ⚡ **Pro Tip:** `uv sync` instala **exatamente** as versões do `uv.lock` para consistency total entre ambientes!

---

## 🏗️ **Project Creation & Setup**

### New Project Workflow
```bash
# Criar novo projeto
uv init my-analytics-project --python 3.12
cd my-analytics-project

# Estrutura automática criada:
# my-analytics-project/
# ├── pyproject.toml     # Config + dependencies
# ├── README.md
# ├── .python-version    # Python version requirement  
# └── src/               # Source code directory
```

### Existing Project Setup
```bash
# Converter projeto existente
cd existing-project
uv init                        # Cria pyproject.toml
uv add -r requirements.txt     # Migra dependencies
```

---

## 📦 **Advanced Dependency Management**

### Core Dependencies
```bash
# Data Science stack essencial
uv add pandas numpy matplotlib seaborn plotly
uv add jupyter sqlalchemy psycopg2-binary

# Analytics avançadas
uv add scikit-learn scipy statsmodels
uv add great-expectations pandas-profiling
```

### Development Dependencies
```bash
# Quality tools (dev only)
uv add --dev pytest pytest-cov black ruff mypy
uv add --dev pre-commit jupyter-lab ipykernel

# Grouping dependencies by purpose
uv add --group test pytest pytest-mock
uv add --group lint ruff black mypy
uv add --group docs mkdocs mkdocs-material
```

### Smart Dependency Resolution
```bash
# Specific versions com constraints
uv add pandas>=2.0.0 "numpy<2.0"
uv add tensorflow==2.13.0

# Add com extras
uv add "sqlalchemy[postgresql,mysql]"
uv add "pandas[all]"

# Conditional dependencies
uv add --platform linux psutil
uv add --python ">=3.10" some-package
```

---

## 🐍 **Python Version Management**

### Multi-Python Setup
```bash
# Instalar versões específicas
uv python install 3.11 3.12 3.13

# Listar versões disponíveis
uv python list

# Projeto com versão específica
uv init --python 3.12 my-project
cd my-project

# Trocar versão em projeto existente  
uv python pin 3.11
uv sync  # Reinstala com nova versão
```

### Version Pinning Strategy
```toml
# pyproject.toml - Flexible constraint
[project]
requires-python = ">=3.11"

# .python-version - Exact version for team
3.12.0
```

---

## ⚡ **High-Performance Workflows**

### Execution Patterns
```bash
# Execução direta (sem ativar venv)
uv run python analysis.py
uv run jupyter lab
uv run pytest tests/
uv run mypy src/

# Temporary packages (não adiciona ao projeto)
uv run --with requests --with beautifulsoup4 python scraper.py

# Scripts com dependencies inline
uv run --with pandas --with matplotlib python data_viz.py

# Cached environments para performance
uv cache clean  # Limpar cache se necessário
```

### Parallel Development
```bash
# Multiple environments para different features
uv venv feature-a --python 3.11
uv venv feature-b --python 3.12

# Context switching
uv venv use feature-a
uv add pandas
uv venv use feature-b  
uv add polars  # Alternative to pandas
```

---

## 🔒 **Reproducible Builds**

### Lock File Management
```bash
# Generate precise lockfile
uv lock                        # Cria/atualiza uv.lock

# Sync from lockfile (production)
uv sync --frozen               # Fail se não está em sync com lock
uv sync --locked               # Use exact versions do lock

# Upgrade strategies
uv sync --upgrade              # Upgrade all within constraints
uv sync --upgrade-package pandas  # Upgrade specific package
```

### Cross-Platform Consistency
```bash
# Export para outros gestores
uv export --format requirements-txt --output-file requirements.txt
uv export --format requirements-txt --no-dev --output-file requirements.txt

# Import de outros formatos
uv add -r requirements.txt
uv add -r pyproject.toml       # Outro projeto uv
```

---

## 🚀 **Performance Optimization**

### Speed Comparisons
| Operação | pip | conda | **UV** | Speedup |
|----------|-----|-------|--------|---------|
| Install 50 packages | 45s | 60s | **2s** | **22-30x** |
| Resolve conflicts | 30s | 40s | **1s** | **30-40x** |
| Create venv | 8s | 15s | **0.3s** | **25-50x** |

### Cache Optimization
```bash
# UV usa cache automático e inteligente
uv cache info                  # Ver estatísticas de cache
uv cache clean                 # Limpar quando necessário
uv cache prune                 # Limpar apenas entries antigas

# Network optimization
UV_INDEX_URL=https://pypi.org/simple/  # Custom index
UV_EXTRA_INDEX_URL=https://my-private-index/  # Private packages
```

---

## 🔧 **Advanced Configuration**

### pyproject.toml Optimization
```toml
[project]
name = "my-data-project"
version = "0.1.0"
description = "Analytics project with UV"
requires-python = ">=3.11"
dependencies = [
    "pandas>=2.0.0",
    "numpy",
    "matplotlib",
]

[project.optional-dependencies]
dev = ["pytest", "black", "ruff"]
analysis = ["scikit-learn", "statsmodels"]
viz = ["plotly", "seaborn", "bokeh"]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.uv]
dev-dependencies = [
    "pytest>=6.0",
    "ruff>=0.1.0",
]

# Custom sources
[[tool.uv.sources]]
name = "pytorch"
url = "https://download.pytorch.org/whl/cpu"
```

### Environment Variables
```bash
# Configuration via environment
export UV_PYTHON_PREFERENCE=only-system  # Use system Python
export UV_RESOLUTION=highest             # Prefer newest versions  
export UV_NO_CACHE=1                     # Disable cache
export UV_CONSTRAINT=constraints.txt     # Global constraints
```

---

## 📊 **Data Science Integration**

### Jupyter Setup
```bash
# Jupyter com UV (método recomendado)
uv add jupyter ipykernel
uv run jupyter lab

# Kernel registration
uv run python -m ipykernel install --user --name myproject

# JupyterLab extensions
uv add --dev jupyterlab-git jupyterlab-code-formatter
uv run jupyter labextension install @jupyter-widgets/jupyterlab-manager
```

### Common Stacks
```bash
# Data Science
uv add pandas numpy scipy matplotlib seaborn plotly
uv add scikit-learn jupyter ipykernel

# Machine Learning  
uv add tensorflow torch torchvision
uv add xgboost lightgbm catboost

# Data Engineering
uv add sqlalchemy psycopg2-binary pymongo
uv add apache-airflow dask prefect
```

---

## 🐛 **Troubleshooting & Migration**

### Common Issues
```bash
# Dependency conflicts
uv lock --resolution=lowest-direct  # Try minimal versions
uv add package --resolution=highest  # Force newest

# Python not found
uv python install 3.12              # Install missing Python
uv python pin 3.12                  # Set for project

# Build errors
UV_BUILD_BACKEND=setuptools uv add problematic-package
uv add --no-build problematic-package  # Use wheels only
```

### Migration from Other Tools
```bash
# From pip
uv add -r requirements.txt
uv add -r dev-requirements.txt --dev

# From conda
# Export first: conda env export > environment.yml
# Then manually migrate to pyproject.toml

# From poetry  
uv add $(cat pyproject.toml | grep -A 100 "\[tool.poetry.dependencies\]" | grep "=" | cut -d'"' -f1)
```

---

## 📋 **Best Practices Checklist**

### ✅ **Essential Do's**
- **Always commit** `pyproject.toml` and `uv.lock`
- **Add `.venv/` to `.gitignore`**
- **Use `uv sync` after git clone**
- **Pin Python version** com `.python-version`
- **Group dependencies** logically (dev, test, analysis)
- **Use `uv run`** para consistency

### ❌ **Common Don'ts**
- Don't install packages globally depois de usar UV
- Don't mix pip and uv no mesmo projeto
- Don't commit `.venv/` directory
- Don't usar `pip install` dentro de UV projects
- Don't esquecer `uv lock` antes de deploy

### 🔄 **Team Workflows**
```bash
# Team member workflow
git clone repo && cd repo
uv sync                    # Install exact versions
uv run pytest            # Run tests  
uv add new-dependency     # Add features
uv lock && git commit     # Lock and commit
```

---

## 🚀 **Advanced Use Cases**

### Monorepo Management
```bash
# Multiple projects in workspace
uv workspace add projects/api
uv workspace add projects/ml-model
uv workspace add projects/dashboard

# Cross-project dependencies
uv add --editable ../shared-utils
```

### CI/CD Integration
```yaml
# GitHub Actions example
- name: Set up UV
  uses: astral-sh/setup-uv@v1
  with:
    version: "latest"

- name: Install dependencies
  run: uv sync --frozen

- name: Run tests
  run: uv run pytest
```

### Docker Integration
```dockerfile
# Dockerfile with UV
FROM python:3.12-slim
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv
COPY pyproject.toml uv.lock ./
RUN uv sync --frozen --no-install-project
COPY . .
RUN uv sync --frozen
ENTRYPOINT ["uv", "run", "python", "main.py"]
```

---

> 🔥 **Bottom Line:** UV é **game-changing** para Python development. Uma vez que experimentes, nunca mais voltarás ao pip tradicional!