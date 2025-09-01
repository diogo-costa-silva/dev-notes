# 🚀 Claude Code - Complete Development Guide

Configuração completa, workflows avançados e best practices para maximizar produtividade com Claude Code.

---

## 🎯 **Essential Commands & Shortcuts**

### Basic Commands
```bash
# Launch Claude Code
claude

# Launch with dangerous mode (skip permissions)
claude --dangerously-skip-permissions
```

### Interactive Commands
```bash
# Inside Claude Code terminal
/mcp                    # Ver servidores MCP registados
/status                 # Ver contexto atual
/model opus            # Switch para Opus (planning)
/model sonnet          # Switch para Sonnet (implementation)
/model haiku           # Switch para Haiku (quick tasks)
/clear                 # Reset context (reduce hallucinations)
```

### Plan Mode
- **Shift + Tab (2x)** → Enter Plan Mode
- Use para planning complexo antes de implementation
- Better to over-plan than under-plan

---

## 🛡️ **Production Guidelines & Rules**

### Core Workflow
1. **Think through problem** → Read codebase → Write plan to tasks/todo.md
2. **Create todo list** with checkable items
3. **Check plan** with team/lead before starting
4. **Execute todo items** marking complete as you go
5. **High-level explanations** at each step
6. **Keep changes simple** → Impact minimal code
7. **Add review section** to todo.md with summary
8. **All outputs in English** (code, docs, comments, variables)

### Model Selection Strategy
- **Opus** → Planning, complex reasoning, architecture
- **Sonnet** → Code execution, implementation, debugging
- **Haiku** → Quick tasks, simple queries, rapid iteration

---

## ⚡ **Performance & Security**

### Dangerous Mode (Development Only)
```bash
# Add to shell profile (~/.bashrc, ~/.zshrc)
alias claude='claude --dangerously-skip-permissions'
```
**⚠️ Only use in trusted development environments!**

### Context Management
- **Use `/clear` regularly** → Prevent hallucinations
- **Smaller contexts** → Lower costs
- **Fresh context** → Better accuracy

### Security Workflow
1. **Plan mode** → Security approach planning
2. **Execute** → Implement planned changes  
3. **Security review** → Comprehensive security check

#### Security Review Prompt:
```
Please check through all the code you just wrote and make sure it follows security best practices. Make sure there are no sensitive information in the frontend and there are no vulnerabilities that can be exploited.
```

---

## 🗄️ **MySQL MCP Database Integration**

### Prerequisites
- Claude Code installed
- MySQL running locally (`brew install mysql`)
- Python 3.11+ with uv (`curl -LsSf https://astral.sh/uv/install.sh | sh`)

### Setup Isolated Environment
```bash
mkdir -p ~/.mcp-envs/mysql
cd ~/.mcp-envs/mysql

uv venv
source .venv/bin/activate

# Ensure pip in venv
python -m ensurepip --upgrade
python -m pip install --upgrade pip

# Install MySQL MCP server
pip install git+https://github.com/designcomputer/mysql_mcp_server.git
# or
pip install mysql-mcp-server
```

### Secure Credentials Configuration

#### Option A: .env file
```bash
cat > ~/.mcp-envs/mysql/.env <<'ENV'
MYSQL_HOST=127.0.0.1
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=your_password_here
MYSQL_DATABASE=information_schema
ENV
```

#### Option B: macOS Keychain (Recommended)
```bash
# Store password in Keychain once
security add-generic-password -a "$USER" -s mysql_root -w 'your_password'

# .env without password
cat > ~/.mcp-envs/mysql/.env <<'ENV'
MYSQL_HOST=127.0.0.1
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=
MYSQL_DATABASE=information_schema
ENV
```

### MCP Server Wrapper
Create `~/bin/mysql-mcp-env.sh`:
```bash
#!/usr/bin/env bash
set -euo pipefail

LOG_TAG="[mysql-mcp-env]"

# Load .env if exists
if [ -f "$HOME/.mcp-envs/mysql/.env" ]; then
  set -a
  . "$HOME/.mcp-envs/mysql/.env"
  set +a
fi

# Fallback to Keychain if no password
if [ "${MYSQL_PASSWORD:-}" = "" ]; then
  if PW="$(security find-generic-password -a "$USER" -s mysql_root -w 2>/dev/null)"; then
    export MYSQL_PASSWORD="$PW"
  else
    echo "$LOG_TAG ERROR: No password found" >&2
    exit 1
  fi
fi

# Set defaults
: "${MYSQL_HOST:=127.0.0.1}"
: "${MYSQL_PORT:=3306}"
: "${MYSQL_USER:?Missing MYSQL_USER}"
: "${MYSQL_DATABASE:?Missing MYSQL_DATABASE}"

export MYSQL_HOST MYSQL_PORT MYSQL_USER MYSQL_PASSWORD MYSQL_DATABASE

# Execute MCP server
EXEC="$HOME/.mcp-envs/mysql/.venv/bin/mysql_mcp_server"
if [ ! -x "$EXEC" ]; then
  echo "$LOG_TAG ERROR: Binary not found at $EXEC" >&2
  exit 1
fi

exec "$EXEC"
```

```bash
chmod +x ~/bin/mysql-mcp-env.sh
```

### Register MCP with Claude Code
```bash
# Remove old configs
claude mcp remove mysql_rw_films -s user 2>/dev/null || true

# Add new server
claude mcp add --scope user mysql_multi_rw -- ~/bin/mysql-mcp-env.sh

# Verify
claude mcp list
claude mcp get mysql_multi_rw
```

### Usage in Claude Code
```bash
claude
```

In Claude Code REPL:
```sql
SHOW DATABASES;
USE your_database;
SHOW TABLES;
SELECT COUNT(*) FROM your_table;
DESCRIBE your_table;
```

### Security Best Practices
- Never commit `.env` → Add to `.gitignore`
- Use Keychain for passwords
- Create dedicated MySQL user for Claude:
  ```sql
  CREATE USER 'claude'@'localhost' IDENTIFIED BY 'secure_password';
  GRANT SELECT, INSERT, UPDATE, DELETE ON *.* TO 'claude'@'localhost';
  FLUSH PRIVILEGES;
  ```

---

## 🎨 **Advanced Usage Patterns**

### Effective Image Usage
1. **UI Inspiration** → Show mockups or design references
2. **Bug Fixing** → Screenshots of errors or UI issues
3. **Architecture Diagrams** → Visual system design

### Learning from Claude
#### Learning Workflow:
1. **Plan mode** → Plan implementation
2. **Execute** → Build feature/fix
3. **Security checks** → Ensure safety
4. **Deep dive** → Understand what was built

#### Learning Prompt:
```
Please explain the functionality and code you just built out in detail. Walk me through what you changed and how it works. Act like you're a senior engineer teaching me code.
```

### Productive Wait Time
Instead of doom scrolling during AI processing:

#### Productive Activities:
- **Code review** → Review Claude's implementation
- **Architecture planning** → Design next features
- **Documentation** → Update README/comments
- **Testing strategy** → Plan test cases
- **Refactoring** → Identify improvement opportunities

#### Conversation Starter:
```
When I am coding with AI there are long breaks between commands. I'd like to use this time to chat with you and generate new ideas, reflect on businesses and content. What could be the best way for us to use this chat productively?
```

---

## 🔄 **Git Integration & Savepoints**

### Savepoint Strategy
- **Git commits** for regular savepoints
- **Branches** for experimental features  
- **Push frequently** to avoid work loss

```bash
git add .
git commit -m "Save progress: feature description"
git push
```

### .gitignore for MCP
```bash
# MCP local setup
.mcp-envs/
*.env

# AI-related
.aiignore
credentials/
secrets/
```

---

## 🚨 **Troubleshooting**

### MCP Connection Issues
```bash
# Test MySQL connection
mysql -uroot -ppassword -h127.0.0.1 -P3306 -e "SELECT 1;"

# Check MCP server status
claude mcp list

# Debug wrapper script
bash -x ~/bin/mysql-mcp-env.sh
```

### Common Issues
- **"Trust folder" prompts** → Run in project directory, not `~/.mcp-envs`
- **"Missing database config"** → Check `.env` variables or Keychain
- **Connection refused** → Ensure MySQL is running: `brew services start mysql`

### Update MCP Library
```bash
source ~/.mcp-envs/mysql/.venv/bin/activate
pip install -U mysql_mcp_server
```

---

## 📋 **Daily Workflow Checklist**

### Morning Setup
- [ ] Start Claude Code: `claude --dangerously-skip-permissions`
- [ ] Check MCP status: `/mcp`
- [ ] Verify MySQL connection: `SHOW DATABASES;`
- [ ] Review yesterday's todos

### Development Cycle
- [ ] **Plan Mode** (Shift + Tab 2x) → Use Opus for planning
- [ ] Create detailed todo list
- [ ] Switch to Sonnet: `/model sonnet`
- [ ] Execute implementation
- [ ] Regular savepoints: `git commit`
- [ ] Security review
- [ ] Documentation update

### End of Day
- [ ] Final git push
- [ ] Update project documentation
- [ ] Clean context: `/clear`
- [ ] Review achievements

---

> 💡 **Pro Tip:** The combination of plan mode + model switching + MCP database integration creates an incredibly powerful development environment. Start with planning in Opus, implement in Sonnet, and validate with direct database queries!