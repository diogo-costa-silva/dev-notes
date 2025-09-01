# 🔄 Python Complete Workflow - From Setup to Deployment

Workflow completo para projetos Python modernos usando UV, Git e deployment automático. Do setup inicial ao deployment em produção.

---

## 🚀 **Project Creation (New Projects)**

### 1. Initial Setup
```bash
# Criar diretório e navegar
mkdir my-analytics-project
cd my-analytics-project

# Configurar Git
git init
git config user.name "Your Name"
git config user.email "your.email@example.com"
```

### 2. Python Environment with UV
```bash
# Instalar Python version específica
uv python install 3.12

# Inicializar projeto UV
uv init --python 3.12

# Estrutura automática criada:
# my-analytics-project/
# ├── pyproject.toml
# ├── README.md  
# ├── .python-version
# └── src/
```

### 3. Core Dependencies Installation
```bash
# Data science stack essencial
uv add pandas numpy matplotlib seaborn plotly
uv add jupyter sqlalchemy psycopg2-binary

# Development tools
uv add --dev pytest black ruff mypy pre-commit

# Specialized dependencies por use case
uv add scikit-learn  # Machine learning
uv add fastapi uvicorn  # API development
uv add streamlit  # Quick dashboards
```

### 4. Project Structure Setup
```bash
# Criar estrutura padrão
mkdir -p {data/{raw,processed,interim},notebooks,scripts,tests,docs}
touch {main.py,.env.example,.gitignore}

# Estrutura completa:
# my-project/
# ├── data/
# │   ├── raw/          # Original data
# │   ├── processed/    # Clean data
# │   └── interim/      # Intermediate steps
# ├── notebooks/        # Jupyter exploration
# ├── scripts/          # Utility scripts
# ├── src/              # Source code
# ├── tests/            # Test files
# ├── docs/             # Documentation
# ├── main.py           # Entry point
# ├── pyproject.toml    # Config + dependencies
# ├── uv.lock           # Locked versions
# └── .env.example      # Environment template
```

---

## 📋 **Essential Configuration Files**

### pyproject.toml (Enhanced)
```toml
[project]
name = "my-analytics-project"
version = "0.1.0"
description = "Data analytics project with modern Python stack"
authors = [{name = "Your Name", email = "your.email@example.com"}]
requires-python = ">=3.11"
dependencies = [
    "pandas>=2.0.0",
    "numpy",
    "matplotlib",
    "seaborn",
]

[project.optional-dependencies]
dev = ["pytest", "black", "ruff", "mypy", "pre-commit"]
ml = ["scikit-learn", "xgboost", "lightgbm"] 
api = ["fastapi", "uvicorn", "pydantic"]
dashboard = ["streamlit", "plotly", "dash"]

[project.scripts]
start = "my_project.main:main"
test = "pytest"

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.ruff]
line-length = 88
target-version = "py311"

[tool.black]
line-length = 88
target-version = ['py311']

[tool.mypy]
python_version = "3.11"
strict = true
```

### .gitignore (Complete)
```bash
# UV/Python
.venv/
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg

# Environment variables
.env
.env.local
.env.production

# Data files (add your patterns)
data/raw/*
data/processed/*
!data/raw/.gitkeep
!data/processed/.gitkeep

# Jupyter
.ipynb_checkpoints
*/.ipynb_checkpoints/*

# IDEs
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
.DS_Store?
._*
Thumbs.db

# Logs
*.log
logs/

# Testing
.coverage
.pytest_cache/
htmlcov/

# Documentation
docs/_build/
```

---

## 👥 **Team Onboarding (Cloning Projects)**

### For UV Users (Recommended)
```bash
# 1. Clone repository
git clone <repository-url>
cd <project-name>

# 2. Setup environment automatically
uv sync                         # Installs exact versions from uv.lock
uv run python main.py           # Run without activating venv

# 3. Development workflow
uv run jupyter lab              # Start Jupyter
uv run pytest                   # Run tests
uv run black src/               # Format code
```

### For Traditional pip Users
```bash
# 1. Clone repository
git clone <repository-url>  
cd <project-name>

# 2. Manual setup
python -m venv .venv
source .venv/bin/activate       # macOS/Linux
# .venv\Scripts\activate        # Windows

# 3. Install dependencies
pip install -r requirements.txt
python main.py                  # Run application
```

---

## 🧪 **Development Workflow**

### Daily Development
```bash
# Morning routine
git pull origin main
uv sync                         # Sync any new dependencies

# Development cycle
uv run jupyter lab              # Exploration
# ... code development ...
uv run pytest tests/           # Test changes
uv run black src/              # Format code
uv run ruff check src/         # Lint code
uv run mypy src/               # Type check

# Commit workflow  
git add .
git commit -m "feat: add new analysis module"
git push origin feature-branch
```

### Adding New Dependencies
```bash
# Add production dependency
uv add requests beautifulsoup4

# Add development dependency
uv add --dev pytest-mock pytest-cov

# Add optional dependency group
uv add --group ml scikit-learn xgboost

# Lock dependencies
uv lock

# Export for compatibility
uv export --format requirements-txt > requirements.txt

# Commit dependency changes
git add pyproject.toml uv.lock requirements.txt
git commit -m "deps: add web scraping libraries"
```

---

## 🔧 **Quality & Testing Setup**

### Pre-commit Hooks
```bash
# Install pre-commit
uv add --dev pre-commit

# Create .pre-commit-config.yaml
cat > .pre-commit-config.yaml << 'EOF'
repos:
  - repo: https://github.com/psf/black
    rev: 23.7.0
    hooks:
      - id: black
        
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.0.284
    hooks:
      - id: ruff
      
  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.5.1
    hooks:
      - id: mypy
        additional_dependencies: [types-requests]
EOF

# Install hooks
uv run pre-commit install
```

### Testing Setup
```bash
# Test structure
mkdir -p tests/{unit,integration,fixtures}
touch tests/{__init__.py,conftest.py}

# conftest.py example
cat > tests/conftest.py << 'EOF'
import pytest
import pandas as pd
from pathlib import Path

@pytest.fixture
def sample_data():
    return pd.DataFrame({
        'id': [1, 2, 3, 4, 5],
        'value': [10, 20, 30, 40, 50],
        'category': ['A', 'B', 'A', 'C', 'B']
    })

@pytest.fixture
def data_dir():
    return Path(__file__).parent / "fixtures"
EOF

# Run tests with coverage
uv run pytest --cov=src tests/
uv run pytest --cov=src --cov-report=html tests/  # HTML report
```

---

## 🚀 **Deployment Strategies**

### 1. Container Deployment (Docker)
```dockerfile
# Dockerfile
FROM python:3.12-slim

# Install UV
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv

# Set working directory
WORKDIR /app

# Copy dependency files
COPY pyproject.toml uv.lock ./

# Install dependencies
RUN uv sync --frozen --no-install-project

# Copy application code
COPY . .

# Install project
RUN uv sync --frozen

# Run application
ENTRYPOINT ["uv", "run", "python", "main.py"]
```

### 2. Cloud Functions (AWS Lambda)
```bash
# Build deployment package
uv export --format requirements-txt > requirements.txt
pip install -r requirements.txt -t package/
cp -r src/* package/
cd package && zip -r ../deployment.zip .

# Deploy with AWS CLI
aws lambda update-function-code \
    --function-name my-function \
    --zip-file fileb://deployment.zip
```

### 3. Traditional Server Deployment
```bash
# Production server setup
python -m venv /opt/myapp/.venv
source /opt/myapp/.venv/bin/activate
pip install -r requirements.txt

# Systemd service
sudo tee /etc/systemd/system/myapp.service << 'EOF'
[Unit]
Description=My Python App
After=network.target

[Service]
Type=simple
User=myapp
WorkingDirectory=/opt/myapp
Environment=PATH=/opt/myapp/.venv/bin
ExecStart=/opt/myapp/.venv/bin/python main.py
Restart=always

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl enable myapp
sudo systemctl start myapp
```

---

## 🔄 **CI/CD Pipeline**

### GitHub Actions
```yaml
# .github/workflows/main.yml
name: CI/CD Pipeline

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.11, 3.12]
        
    steps:
    - uses: actions/checkout@v4
    
    - name: Set up UV
      uses: astral-sh/setup-uv@v1
      with:
        version: "latest"
        
    - name: Set up Python ${{ matrix.python-version }}
      run: uv python install ${{ matrix.python-version }}
      
    - name: Install dependencies
      run: uv sync --all-extras
      
    - name: Run tests
      run: uv run pytest --cov=src tests/
      
    - name: Upload coverage
      uses: codecov/codecov-action@v3
      
  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - uses: actions/checkout@v4
    - name: Deploy to production
      run: echo "Deploy steps here"
```

---

## 📊 **Project Templates by Use Case**

### Data Science Project
```bash
# Specialized data science setup
uv add pandas numpy scipy matplotlib seaborn plotly
uv add jupyter ipykernel scikit-learn statsmodels
uv add --dev pytest black ruff mypy

# Additional structure
mkdir -p {models,results,reports}
touch {exploratory_analysis.ipynb,model_training.py,evaluation.py}
```

### API Development
```bash
# FastAPI project setup
uv add fastapi uvicorn pydantic python-multipart
uv add sqlalchemy psycopg2-binary alembic
uv add --dev pytest httpx

# API structure
mkdir -p {app/{models,routers,services},tests/{unit,integration}}
touch {app/__init__.py,app/main.py,app/database.py}
```

### CLI Application  
```bash
# CLI tools setup
uv add click rich typer
uv add --dev pytest pytest-click

# CLI structure
mkdir -p {cli/{commands,utils},tests}
touch {cli/__init__.py,cli/main.py,cli/commands/__init__.py}
```

---

## 📈 **Monitoring & Maintenance**

### Health Checks
```bash
# Dependency updates
uv sync --upgrade                   # Update all packages
uv sync --upgrade-package pandas    # Update specific package

# Security audit
uv audit                            # Check for vulnerabilities
uv export | pip-audit -r -         # Alternative security check

# Performance monitoring  
uv run python -m cProfile main.py  # Profile performance
uv run memory_profiler main.py     # Memory usage
```

### Documentation Generation
```bash
# Auto-documentation
uv add --dev sphinx sphinx-rtd-theme
uv run sphinx-quickstart docs/
uv run sphinx-build -b html docs/ docs/_build/
```

---

> 🎯 **Key Takeaway:** Este workflow garante projetos Python **reproduzíveis, escaláveis e maintíveis**. Adapta conforme as necessidades específicas do teu projeto!