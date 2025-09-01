# 🐍 Python Development

Setup moderno e workflows para desenvolvimento Python usando as melhores práticas e ferramentas atuais.

## ⚡ **UV Package Manager**

- **[UV Complete Guide](uv.md)** - Gestor de pacotes Python ultra-rápido, substituto moderno do pip/conda
- **[Python Setup](python.md)** - Configuração completa do ambiente Python
- **[Complete Workflow](python_workflow.md)** - Do setup ao deploy, workflow completo

## 🏗️ **Project Structure & Best Practices**

### Modern Python Stack
- **UV** para gestão de dependências e ambientes virtuais
- **Ruff** para linting e formatting ultra-rápido
- **pytest** para testing robusto
- **pre-commit hooks** para qualidade de código
- **GitHub Actions** para CI/CD

### Development Workflow
```bash
# Inicializar projeto moderno
uv init --python 3.12
uv add fastapi uvicorn pytest ruff

# Desenvolvimento
uv run python main.py
uv run pytest
uv run ruff check .
```

## 🚀 **Performance & Optimization**

- **Type hints** para melhor maintainability
- **Async/await** patterns para concorrência
- **Pydantic** para validation de dados
- **FastAPI** para APIs modernas e rápidas

## 📦 **Package Management**

### UV Advantages
- **10-100x mais rápido** que pip/conda
- **Lock files** automáticos para builds reproduzíveis
- **Python version management** integrado
- **Virtual environments** automáticos

### Migration from pip/conda
```bash
# Migrar requirements.txt
uv add -r requirements.txt

# Migrar environment.yml
uv python install 3.12
uv venv --python 3.12
```

## 🧪 **Testing & Quality**

- **pytest** com fixtures e parametrização
- **Coverage reports** integrados
- **Ruff** para linting moderno
- **pre-commit** para validações automáticas

---

> 🔥 **Pro Tip:** UV revoluciona o desenvolvimento Python - experimenta e nunca mais voltarás ao pip tradicional!