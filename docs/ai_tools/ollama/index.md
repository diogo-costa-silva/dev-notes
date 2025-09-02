# 🦙 Ollama Local AI

Setup completo para desenvolvimento com modelos de linguagem locais. Privacy-first development com performance otimizada para macOS.

## 📋 **Conteúdo Disponível**

### [⚡ Ollama Complete Setup Guide](ollama.md)

**Guia completo para desenvolvimento local com IA**

- ✅ Setup otimizado via Homebrew
- ✅ Gestão avançada de modelos (CodeLlama, DeepSeek, Qwen)
- ✅ Integração completa com VS Code Continue
- ✅ Performance optimization e resource management
- ✅ Privacy e security best practices
- ✅ Troubleshooting e workflows diários

### [🔧 VS Code Integration Guide](ollama_local_setup.md)

**Setup detalhado para integração VS Code + Continue**

- ✅ Verificação de sistema e dependencies
- ✅ Configuração paso-a-passo do Continue
- ✅ Automação com VS Code tasks
- ✅ Troubleshooting de conexões
- ✅ Model management e optimization

---

## 🚀 **Quick Start**

### Instalação Básica

```bash
# Install via Homebrew (recommended)
brew install ollama

# Remove conflicting app version
sudo rm -rf /Applications/Ollama.app

# Start as service
brew services start ollama

# Install essential models
ollama pull llama3:8b
ollama pull codellama:34b
ollama pull deepseek-coder:33b
```

### VS Code Integration

1. Install Continue extension in VS Code
2. Configure `~/Library/Application Support/Continue/config.json`
3. Test connection in Continue panel

---

## 🎯 **Casos de Uso Principais**

### Code Development
- **Autocompletion** em tempo real via Continue
- **Code generation** para features complexas
- **Refactoring** e optimization suggestions
- **Code review** e bug detection

### Privacy-First Workflows
- **Código proprietário** nunca sai da máquina
- **Desenvolvimento offline** completo
- **Zero telemetry** ou data collection
- **Air-gapped environments** suportados

### Model Strategy
- **qwen2.5-coder:1.5b** → Fast autocomplete
- **codellama:34b** → Quality code generation
- **deepseek-coder:33b** → Comprehensive analysis
- **llama3:8b** → General chat e explanation

---

## 🔧 **Terminal Usage**

### Interactive Sessions

```bash
# General chat
ollama run llama3:8b

# Code assistance
ollama run codellama:34b

# Quick coding tasks
ollama run qwen2.5-coder:1.5b
```

### One-shot Queries

```bash
# Code generation
ollama run deepseek-coder "create a REST API with FastAPI"

# Code review
ollama run codellama "optimize this SQL query for performance"

# Explanation
ollama run llama3 "explain this Python function"
```

---

## 🔒 **Privacy & Security**

### Complete Local Control
- **Models stored** em `~/.ollama/models`
- **Zero external API calls** durante inference
- **Complete offline capability**
- **Network isolation** opcional

### Performance Optimization
- **Hardware-aware** model selection
- **Memory management** automático
- **Metal acceleration** em Apple Silicon
- **Resource monitoring** integrado

---

> 🔐 **Privacy Advantage:** Com Ollama, o teu código proprietário nunca sai da tua máquina. Perfeito para projetos sensíveis, trabalho confidencial e ambientes isolados!