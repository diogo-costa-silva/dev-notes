# 🤖 AI Tools for Development

Ferramentas de inteligência artificial otimizadas para desenvolvimento, com foco nas configurações práticas e workflows testados.

## 🔧 **Core Tools**

### [🚀 Claude Code](claude_code_complete.md)
**Configuração completa e workflows avançados para Claude Code**
- ✅ Comandos essenciais e shortcuts
- ✅ Guidelines e regras de produtividade
- ✅ MCP setup para bases de dados MySQL
- ✅ Plan mode e model switching strategies
- ✅ Security checks e best practices

### [🦙 Ollama Local AI](ollama_local_setup.md) 
**Setup completo para modelos de linguagem locais**
- ✅ Installation e configuration via Homebrew
- ✅ Model management (CodeLlama, Mistral, Phi3)
- ✅ VS Code integration com Continue
- ✅ Terminal workflows e automation
- ✅ Privacy-first local development

### [💬 ChatGPT MCPs](chatgpt_mcps.md)
**Model Context Protocol setup para ChatGPT**
- ✅ MCP server configuration
- ✅ Database connectivity setup
- ✅ Context management strategies
- ✅ Integration workflows

---

## ⚡ **Quick Start Workflows**

### Claude Code Daily Workflow
```bash
# Start Claude Code
claude --dangerously-skip-permissions

# Enter plan mode
# Press Shift + Tab (2x)

# Essential commands
/mcp                    # Ver servidores MCP registados
/status                 # Ver contexto atual
/model opus            # Switch para planning
/model sonnet          # Switch para implementation
/clear                 # Reset context
```

### Ollama Local Development
```bash
# Start Ollama service
brew services start ollama

# Essential models
ollama pull llama3:8b
ollama pull codellama:34b
ollama pull deepseek-coder:33b

# Interactive coding
ollama run codellama
ollama run deepseek-coder
```

---

## 🛡️ **Security & Privacy**

### Local vs Cloud Trade-offs
- **Local (Ollama)** → Código proprietário, dados sensíveis
- **Cloud (Claude/ChatGPT)** → Performance superior, features avançadas
- **Hybrid approach** → Local para development, Cloud para planning

### Best Practices
```bash
# Configuration de context limits
export CLAUDE_MAX_TOKENS=4000
export OPENAI_MAX_CONTEXT=8000

# Filter sensitive data
echo "*.env" >> .aiignore
echo "secrets/" >> .aiignore
echo "credentials/" >> .aiignore
```

---

## 🔗 **Integration Patterns**

### Database Connectivity
- **MySQL MCP** para queries diretas via Claude Code
- **PostgreSQL integration** para analytics workflows
- **Local SQLite** para prototyping rápido

### IDE Integration
- **Continue + Ollama** no VS Code para autocomplete local
- **Claude Code** para complex reasoning e architecture
- **GitHub Copilot** para suggestions em tempo real

### Command Line AI
```bash
# GitHub Copilot CLI
gh copilot suggest "create a REST API with FastAPI"
gh copilot explain "docker-compose up -d"

# Ollama queries
ollama run codellama "refactor this Python function"
ollama run deepseek-coder "optimize this SQL query"
```

---

## 🎯 **Use Case Optimization**

### Planning & Architecture
- **Claude Code Opus** → Complex system design
- **Plan mode** → Multi-step implementation breakdown
- **Security reviews** → Vulnerability assessment

### Code Implementation  
- **Claude Code Sonnet** → Feature implementation
- **Ollama CodeLlama** → Local code completion
- **Continue VS Code** → Real-time suggestions

### Code Review & Documentation
- **AI-powered reviews** → Automated PR analysis
- **Documentation generation** → Code explanation
- **Test generation** → Unit test creation

---

## 📊 **Performance Optimization**

### Context Management
- **Use `/clear`** regularmente para reduzir hallucinations
- **Batch similar tasks** para efficiency
- **Savepoints com Git** durante development

### Model Selection
- **Opus** → Planning, architecture, complex reasoning
- **Sonnet** → Implementation, debugging, testing  
- **Haiku** → Simple queries, quick tasks
- **Local models** → Privacy-sensitive code

---

## 🚀 **Advanced Workflows**

### Multi-Agent Development
1. **Claude Opus** → Plan & architect
2. **Claude Sonnet** → Implement features
3. **Local Ollama** → Code completion
4. **Security review** → Final validation

### Database-Driven Development
1. **MCP connection** → Direct database access
2. **Schema analysis** → Structure understanding
3. **Query optimization** → Performance tuning
4. **Data validation** → Quality assurance

---

> 🔐 **Security First:** Para código proprietário, usa sempre modelos locais. Para features avançadas e planning, cloud models com context filtering adequado.