# 🚀 Claude Tools

Ferramentas e configurações para Claude Code e outros produtos Claude, com foco em workflows de desenvolvimento profissional.

## 📋 **Conteúdo Disponível**

### [⚡ Claude Code Complete Guide](claude_code.md)

**Guia completo para maximizar produtividade com Claude Code**

- ✅ Comandos essenciais e shortcuts avançados
- ✅ Plan Mode e estratégias de model switching
- ✅ Integração completa com MySQL via MCP
- ✅ Security workflows e best practices
- ✅ Git integration e savepoint strategies
- ✅ Troubleshooting e configuração de ambiente

---

## 🚀 **Quick Start**

### Comandos Essenciais

```bash
# Iniciar Claude Code
claude --dangerously-skip-permissions

# Comandos interactivos
/mcp                    # Ver servidores MCP
/status                 # Ver contexto atual
/model opus            # Switch para planning
/model sonnet          # Switch para implementation
/clear                 # Reset context
```

### Plan Mode

- **Shift + Tab (2x)** → Entrar em Plan Mode
- Usar para planning complexo antes da implementação
- Melhor over-plan do que under-plan

---

## 🎯 **Workflow de Desenvolvimento**

### Ciclo Diário Recomendado

1. **Plan Mode (Opus)** → Planeamento e arquitetura
2. **Implementation (Sonnet)** → Desenvolvimento de features  
3. **Database Queries (MCP)** → Validação com dados reais
4. **Security Review** → Verificação de boas práticas
5. **Git Savepoints** → Commits regulares

### Seleção de Modelos

- **Opus** → Planning, reasoning complexo, arquitetura
- **Sonnet** → Implementação, debugging, testing
- **Haiku** → Tarefas rápidas, queries simples

---

## 🗄️ **Database Integration**

### MySQL MCP Setup

```bash
# Ambiente isolado
mkdir -p ~/.mcp-envs/mysql
cd ~/.mcp-envs/mysql
uv venv && source .venv/bin/activate

# Instalar servidor MCP
pip install mysql-mcp-server

# Registar com Claude Code
claude mcp add --scope user mysql_multi_rw -- ~/bin/mysql-mcp-env.sh
```

### Uso na Prática

```sql
-- Directo no Claude Code REPL
SHOW DATABASES;
USE your_database;  
SHOW TABLES;
SELECT COUNT(*) FROM users WHERE created_at > '2024-01-01';
```

---

## 🛡️ **Security & Best Practices**

### Guidelines de Produção

- **English only** para código, docs, comments, variáveis
- **Minimal impact** → Alterar o mínimo código necessário
- **Security reviews** obrigatórios após implementação
- **Context management** → Usar `/clear` regularmente

### Security Review Prompt

```
Please check through all the code you just wrote and make sure it follows security best practices. Make sure there are no sensitive information in the frontend and there are no vulnerabilities that can be exploited.
```

---

> 💡 **Pro Tip:** A combinação de Plan Mode + Model Switching + MCP Database Integration cria um ambiente de desenvolvimento extremamente poderoso. Planeia em Opus, implementa em Sonnet, e valida com queries diretas à base de dados!