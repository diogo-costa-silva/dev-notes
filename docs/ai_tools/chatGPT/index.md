# 💬 ChatGPT Tools

Ferramentas e configurações para ChatGPT, incluindo integração com Model Context Protocol (MCP) para workflows avançados de desenvolvimento.

## 📋 **Conteúdo Disponível**

### [🔌 ChatGPT MCPs](chatgpt_mcps.md)

**Setup completo do Model Context Protocol para ChatGPT**

- ✅ Configuração de servidores MCP para bases de dados
- ✅ Integração com sistemas de ficheiros
- ✅ Conectividade API em tempo real
- ✅ Servidores personalizados e scripts de automação
- ✅ Monitorização, debugging e best practices

---

## 🚀 **Quick Start**

### Configuração Básica MCP

```bash
# Instalar ferramentas MCP
npm install -g @anthropic/mcp
python3 -m pip install mcp

# Verificar instalação
mcp --version
```

### Exemplo de Configuração MySQL

```bash
# Criar configuração MySQL MCP
mkdir -p ~/.mcp/servers
cat > ~/.mcp/servers/mysql.json <<'EOF'
{
  "name": "mysql-server",
  "command": "mcp-mysql-server", 
  "args": [
    "--host", "localhost",
    "--port", "3306",
    "--database", "your_database",
    "--user", "your_user",
    "--password-env", "MYSQL_PASSWORD"
  ]
}
EOF

# Registar servidor
mcp register ~/.mcp/servers/mysql.json
```

---

## 🎯 **Casos de Uso Principais**

### Análise de Dados em Tempo Real
- **Queries SQL** diretas através de conversas ChatGPT
- **Visualização** de dados e tendências
- **Reports** automáticos de métricas de negócio

### Gestão de Projetos
- **Acesso a ficheiros** de projeto em tempo real
- **Análise de código** e estrutura
- **Integração** com APIs GitHub/GitLab

### Automação de Workflows
- **Scripts personalizados** através de MCP servers
- **Monitoring** de sistemas e aplicações
- **Notificações** e alertas automatizados

---

> 🔌 **Nota:** Os MCPs transformam o ChatGPT numa interface dinâmica para todo o teu ecossistema de desenvolvimento, permitindo acesso em tempo real a bases de dados, ficheiros e APIs!