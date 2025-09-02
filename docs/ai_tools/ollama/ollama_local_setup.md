# 🦙 Ollama - Complete Local AI Setup Guide

Setup completo para desenvolvimento com modelos de linguagem locais no macOS. Privacy-first development com performance otimizada.

---

## 🚀 **Quick Setup**

### Installation
```bash
# Install via Homebrew (recommended)
brew install ollama

# Remove conflicting App version if exists
sudo rm -rf /Applications/Ollama.app

# Start as service
brew services start ollama

# Verify installation
ollama --version
lsof -iTCP:11434 -sTCP:LISTEN -nP
```

---

## 🔍 **System Verification**

### Pre-Installation Checks
```bash
# System info
sw_vers
uname -m
sysctl -n machdep.cpu.brand_string 2>/dev/null || true

# Dependencies
brew --version && brew doctor
node -v && npm -v               # >= 18 required for Continue
code -v                        # VS Code >= 1.90

# Check for conflicts
which -a ollama
type -a ollama
launchctl list | grep -i ollama || true
```

### Service Management
```bash
# Start/stop service
brew services start ollama
brew services stop ollama
brew services restart ollama

# Check service status
brew services list | grep ollama
curl -s http://localhost:11434/api/version
```

---

## 🧠 **Model Management**

### Essential Models
```bash
# Primary language model
ollama pull llama3:8b

# Coding specialists
ollama pull codellama:34b
ollama pull deepseek-coder:33b

# Lightweight alternatives
ollama pull codellama:7b
ollama pull qwen2.5-coder:1.5b

# Embeddings
ollama pull nomic-embed-text:latest

# General purpose
ollama pull mistral:7b
ollama pull phi3:mini
```

### Model Operations
```bash
# List installed models
ollama list

# Remove models (free space)
ollama rm model_name

# View model details
ollama show model_name
ollama show --modelfile codellama

# Check disk usage
du -sh ~/.ollama/models
```

---

## 💻 **Terminal Usage**

### Interactive Sessions
```bash
# General chat
ollama run llama3:8b
ollama run mistral

# Code assistance
ollama run codellama
ollama run deepseek-coder

# Quick coding tasks
ollama run qwen2.5-coder:1.5b

# Exit: Ctrl + C or /bye
```

### One-shot Queries
```bash
# Code explanation
ollama run codellama "explain this Python function: def factorial(n):"

# Code generation  
ollama run deepseek-coder "create a REST API endpoint in FastAPI"

# Refactoring
ollama run codellama "optimize this SQL query for performance"

# Code review
ollama run deepseek-coder "review this JavaScript code for bugs"
```

---

## 🔧 **VS Code Integration with Continue**

### Extension Setup
1. Open VS Code
2. Extensions → Search "Continue" → Install
3. Restart VS Code

### Configuration
Create/edit `~/Library/Application Support/Continue/config.json`:
```json
{
  "models": [
    {
      "title": "Llama 3 8B",
      "provider": "ollama",
      "model": "llama3:8b",
      "apiBase": "http://localhost:11434",
      "contextLength": 8192,
      "roles": ["chat"]
    },
    {
      "title": "CodeLlama 34B",
      "provider": "ollama", 
      "model": "codellama:34b",
      "apiBase": "http://localhost:11434",
      "contextLength": 16384,
      "roles": ["autocomplete", "edit"]
    },
    {
      "title": "DeepSeek Coder",
      "provider": "ollama",
      "model": "deepseek-coder:33b", 
      "apiBase": "http://localhost:11434",
      "contextLength": 16384,
      "roles": ["autocomplete", "edit"]
    }
  ],
  "tabAutocompleteModel": {
    "title": "Qwen Coder",
    "provider": "ollama",
    "model": "qwen2.5-coder:1.5b",
    "apiBase": "http://localhost:11434"
  }
}
```

### Testing Integration
1. Open Continue panel (`⌘J` → Continue)
2. Test connection: `/model llama3:8b`
3. Try autocomplete: Start typing code
4. Use chat: Ask questions about your code

---

## ⚙️ **Advanced Configuration**

### Performance Optimization
```bash
# Environment variables (add to ~/.zshrc)
export OLLAMA_NUM_PARALLEL=2
export OLLAMA_MAX_LOADED_MODELS=3
export OLLAMA_FLASH_ATTENTION=1

# Model-specific settings
export OLLAMA_KEEP_ALIVE=5m
export OLLAMA_LOAD_TIMEOUT=10m
```

### Custom Model Parameters
Create `Modelfile`:
```dockerfile
# Custom CodeLlama with specific settings
FROM codellama:34b

# Set parameters
PARAMETER temperature 0.1
PARAMETER top_k 40
PARAMETER top_p 0.9
PARAMETER repeat_penalty 1.1

# System message
SYSTEM """
You are an expert software engineer. Provide concise, accurate code solutions.
Focus on best practices, security, and performance.
"""
```

```bash
# Create custom model
ollama create my-codellama -f Modelfile
ollama run my-codellama
```

---

## 🛠️ **Development Workflows**

### VS Code Task Automation
`.vscode/tasks.json`:
```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Start Ollama",
      "type": "shell",
      "command": "brew services start ollama",
      "group": "build",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "panel": "new"
      }
    },
    {
      "label": "Stop Ollama", 
      "type": "shell",
      "command": "brew services stop ollama",
      "group": "build"
    },
    {
      "label": "Ollama Status",
      "type": "shell",
      "command": "curl -s http://localhost:11434/api/tags | jq .",
      "group": "build"
    }
  ]
}
```

### Model Switching Script
Create `~/bin/ollama-switch.sh`:
```bash
#!/bin/bash
MODEL=$1

if [ -z "$MODEL" ]; then
  echo "Available models:"
  ollama list
  echo "Usage: ollama-switch.sh <model-name>"
  exit 1
fi

echo "Switching to $MODEL..."
ollama run $MODEL
```

```bash
chmod +x ~/bin/ollama-switch.sh
ollama-switch.sh codellama
```

---

## 📊 **Performance Monitoring**

### Resource Usage
```bash
# Monitor CPU/Memory during inference
top -pid $(pgrep ollama)

# Check GPU usage (if applicable)
sudo powermetrics --samplers gpu_power -a --hide-cpu-duty-cycle -n 1

# Model loading time
time ollama run codellama "print hello world"
```

### Benchmark Different Models
```bash
# Speed test script
#!/bin/bash
MODELS=("codellama:7b" "codellama:34b" "deepseek-coder:33b")
PROMPT="Write a Python function to calculate fibonacci numbers"

for model in "${MODELS[@]}"; do
  echo "Testing $model..."
  time ollama run $model "$PROMPT" > /dev/null
  echo "---"
done
```

---

## 🔒 **Privacy & Security**

### Data Locality
- **Models stored locally** in `~/.ollama/models`
- **No data sent to external APIs**  
- **Complete offline capability**
- **Network isolation** for sensitive projects

### Security Best Practices
```bash
# Restrict network access (optional)
sudo pfctl -f /etc/pf.conf -e
# Add rule to block ollama external connections

# Monitor network activity
lsof -i -P | grep ollama

# Secure model directory
chmod 750 ~/.ollama/models
```

### Environment Isolation
```bash
# Project-specific model configs
mkdir -p .ollama/
export OLLAMA_MODELS=".ollama/models"

# Use different model versions per project
ollama pull codellama:7b-instruct
```

---

## 🚨 **Troubleshooting**

### Common Issues
```bash
# Service not starting
brew services restart ollama
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.ollama.plist

# Port conflicts
lsof -iTCP:11434 -sTCP:LISTEN
sudo kill -9 $(lsof -ti:11434)

# Permission issues
sudo chown -R $(whoami) ~/.ollama
```

### Debug Mode
```bash
# Verbose logging
OLLAMA_DEBUG=1 ollama serve

# Monitor logs
log stream --predicate 'process == "ollama"' --info

# Check system logs
brew services info ollama
```

### Performance Issues
```bash
# Clear cache
rm -rf ~/.ollama/cache/

# Reduce concurrent models
export OLLAMA_MAX_LOADED_MODELS=1

# Memory monitoring
memory_pressure && echo "Memory pressure detected"
```

---

## 📈 **Optimization Tips**

### Model Selection Strategy
- **Code completion** → `qwen2.5-coder:1.5b` (fast)
- **Code generation** → `codellama:34b` (accurate)  
- **Code review** → `deepseek-coder:33b` (comprehensive)
- **Chat/explanation** → `llama3:8b` (conversational)

### Hardware Optimization
- **8GB RAM** → Use 7B models max
- **16GB RAM** → Mix of 7B/13B models
- **32GB+ RAM** → 34B models for best quality
- **Apple Silicon** → Optimized Metal performance

### Storage Management
```bash
# Model sizes (approximate)
# 7B models:  ~4GB
# 13B models: ~7GB  
# 34B models: ~20GB

# Clean unused models quarterly
ollama list | grep -v "NAME" | awk '{print $1}' | sort
```

---

## 🔄 **Daily Workflow**

### Morning Startup
```bash
# Check service status
brew services list | grep ollama

# Warm up primary models
ollama run codellama:34b "ready" > /dev/null &
ollama run llama3:8b "ready" > /dev/null &
```

### Development Session
1. **VS Code** → Continue extension for autocomplete
2. **Terminal** → Direct model interaction for complex queries
3. **Context switching** → Different models for different tasks

### End of Day
```bash
# Optional: stop service to save resources
brew services stop ollama

# Check disk usage
du -sh ~/.ollama/models
```

---

> 🔐 **Privacy Advantage:** With Ollama, your proprietary code never leaves your machine. Perfect for sensitive projects, confidential work, and air-gapped environments!