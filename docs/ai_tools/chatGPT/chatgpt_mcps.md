# 💬 ChatGPT MCPs - Model Context Protocol Setup

Configuration and setup guide for integrating ChatGPT with Model Context Protocol (MCP) servers for enhanced development workflows.

---

## 🎯 **Overview**

Model Context Protocol (MCP) enables ChatGPT to connect directly to external systems like databases, file systems, and APIs, providing real-time access to dynamic data during conversations.

### Key Benefits
- **Real-time data access** during ChatGPT conversations
- **Database connectivity** for live queries and analysis
- **File system integration** for project context
- **API connectivity** for external service interaction
- **Extensible architecture** for custom integrations

---

## 🔧 **Prerequisites**

```bash
# Required tools
npm install -g @anthropic/mcp
python3 -m pip install mcp

# Verify installation
mcp --version
python3 -m mcp.server --help
```

---

## 🗄️ **Database MCP Setup**

### MySQL MCP Server
```bash
# Install MySQL MCP server
npm install -g @anthropic/mcp-mysql-server

# Create configuration
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
  ],
  "env": {
    "MYSQL_PASSWORD": "your_secure_password"
  }
}
EOF
```

### PostgreSQL MCP Server
```bash
# Install PostgreSQL MCP server
npm install -g @anthropic/mcp-postgres-server

# Configuration
cat > ~/.mcp/servers/postgres.json <<'EOF'
{
  "name": "postgres-server",
  "command": "mcp-postgres-server",
  "args": [
    "--host", "localhost", 
    "--port", "5432",
    "--database", "your_database",
    "--user", "your_user",
    "--password-env", "POSTGRES_PASSWORD"
  ],
  "env": {
    "POSTGRES_PASSWORD": "your_secure_password"
  }
}
EOF
```

---

## 📁 **File System MCP Setup**

### Local File Access
```bash
# Install file system MCP server
npm install -g @anthropic/mcp-filesystem-server

# Configuration
cat > ~/.mcp/servers/filesystem.json <<'EOF'
{
  "name": "filesystem-server",
  "command": "mcp-filesystem-server",
  "args": [
    "--root", "/Users/yourusername/projects",
    "--allowed-extensions", ".js,.py,.md,.json,.sql",
    "--max-file-size", "10MB"
  ]
}
EOF
```

---

## 🌐 **API Integration MCP**

### REST API MCP Server
```bash
# Install REST API MCP server  
npm install -g @anthropic/mcp-rest-server

# Configuration for common APIs
cat > ~/.mcp/servers/github-api.json <<'EOF'
{
  "name": "github-api",
  "command": "mcp-rest-server", 
  "args": [
    "--base-url", "https://api.github.com",
    "--headers", "Authorization=token $GITHUB_TOKEN",
    "--headers", "Accept=application/vnd.github.v3+json"
  ],
  "env": {
    "GITHUB_TOKEN": "your_github_token"
  }
}
EOF
```

---

## ⚙️ **ChatGPT Integration**

### Enable MCP in ChatGPT
1. Open ChatGPT settings
2. Navigate to "Advanced Settings" 
3. Enable "Model Context Protocol"
4. Add server configurations

### Server Registration
```bash
# Register all MCP servers
mcp register ~/.mcp/servers/mysql.json
mcp register ~/.mcp/servers/postgres.json  
mcp register ~/.mcp/servers/filesystem.json
mcp register ~/.mcp/servers/github-api.json

# Verify registration
mcp list-servers
mcp test-server mysql-server
```

---

## 🚀 **Usage Examples**

### Database Queries in ChatGPT
```
Query the database for:
- Show all tables in the database
- Get user count from users table
- Find top 10 products by sales
- Analyze user engagement trends

MCP will execute these as live SQL queries.
```

### File System Operations
```
Access project files:
- Read package.json from current project
- List all Python files in src/ directory  
- Show recent commit history
- Analyze code structure

MCP provides real-time file access.
```

### API Interactions
```
GitHub API operations:
- List my repositories
- Show open pull requests
- Get repository statistics
- Create new issues

MCP connects directly to GitHub API.
```

---

## 🔒 **Security Configuration**

### Environment Variables
```bash
# Secure credential management
export MYSQL_PASSWORD="secure_password"
export POSTGRES_PASSWORD="secure_password"  
export GITHUB_TOKEN="ghp_your_token_here"

# Add to ~/.zshrc or ~/.bashrc
echo 'export MYSQL_PASSWORD="your_password"' >> ~/.zshrc
```

### Access Control
```json
{
  "name": "secure-mysql",
  "command": "mcp-mysql-server",
  "security": {
    "allowed_operations": ["SELECT", "SHOW"],
    "forbidden_tables": ["users", "passwords", "secrets"],
    "rate_limit": "100/hour"
  }
}
```

### Network Security
```bash
# Restrict MCP server access
sudo pfctl -f /etc/pf.conf -e
# Add firewall rules for MCP ports

# Monitor MCP connections
lsof -i | grep mcp
netstat -an | grep :8080
```

---

## 🛠️ **Custom MCP Servers**

### Simple Python MCP Server
```python
#!/usr/bin/env python3
"""
Custom MCP server for project analytics
"""
import json
import sys
from mcp.server import McpServer

app = McpServer()

@app.tool("analyze_project")
def analyze_project(project_path: str):
    """Analyze project structure and metrics"""
    import os
    import glob
    
    stats = {
        "total_files": 0,
        "python_files": 0, 
        "js_files": 0,
        "lines_of_code": 0
    }
    
    for file_path in glob.glob(f"{project_path}/**/*", recursive=True):
        if os.path.isfile(file_path):
            stats["total_files"] += 1
            
            if file_path.endswith('.py'):
                stats["python_files"] += 1
            elif file_path.endswith('.js'):
                stats["js_files"] += 1
                
            # Count lines
            try:
                with open(file_path, 'r') as f:
                    stats["lines_of_code"] += len(f.readlines())
            except:
                pass
    
    return stats

if __name__ == "__main__":
    app.run()
```

### Register Custom Server
```bash
# Make executable
chmod +x custom_mcp_server.py

# Configuration
cat > ~/.mcp/servers/custom-analytics.json <<'EOF'
{
  "name": "custom-analytics",
  "command": "/path/to/custom_mcp_server.py",
  "description": "Custom project analytics MCP server"
}
EOF

# Register
mcp register ~/.mcp/servers/custom-analytics.json
```

---

## 📊 **Monitoring & Debugging**

### Server Health Monitoring
```bash
# Check server status
mcp status

# Test specific server
mcp test-server mysql-server --verbose

# View server logs
tail -f ~/.mcp/logs/mysql-server.log
```

### Debug Configuration
```json
{
  "name": "debug-mysql",
  "command": "mcp-mysql-server",
  "debug": true,
  "log_level": "DEBUG",
  "log_file": "~/.mcp/logs/mysql-debug.log"
}
```

### Performance Monitoring
```bash
# Monitor MCP server resource usage
ps aux | grep mcp
top -p $(pgrep mcp-mysql-server)

# Network monitoring
netstat -an | grep mcp
iftop -i en0 | grep mcp
```

---

## 🚨 **Troubleshooting**

### Common Issues

**Connection Refused**
```bash
# Check if server is running
ps aux | grep mcp-mysql-server

# Restart server
mcp restart mysql-server

# Check configuration
mcp validate-config ~/.mcp/servers/mysql.json
```

**Authentication Errors**
```bash
# Verify credentials
echo $MYSQL_PASSWORD
mysql -u user -p$MYSQL_PASSWORD -h localhost -e "SELECT 1;"

# Reset credentials
mcp update-credentials mysql-server
```

**Performance Issues**
```bash
# Check resource usage
mcp stats mysql-server

# Optimize configuration
mcp optimize mysql-server

# Reduce connection pool
{
  "max_connections": 5,
  "connection_timeout": 30
}
```

---

## 🔄 **Best Practices**

### Configuration Management
- **Version control** MCP configurations
- **Environment-specific** server configs
- **Secure credential** storage
- **Regular health** monitoring

### Development Workflow
1. **Local testing** → Test MCP servers individually
2. **Integration testing** → Verify ChatGPT connectivity  
3. **Performance testing** → Monitor response times
4. **Security review** → Audit access permissions

### Maintenance
```bash
# Weekly maintenance script
#!/bin/bash

# Update MCP servers
npm update -g @anthropic/mcp-*

# Clean logs
find ~/.mcp/logs -name "*.log" -mtime +7 -delete

# Health check
mcp health-check --all

# Backup configurations
cp -r ~/.mcp/servers ~/.mcp/backups/$(date +%Y%m%d)
```

---

## 📈 **Advanced Use Cases**

### Multi-Database Analytics
- **Cross-database queries** via multiple MCP servers
- **Real-time dashboards** through ChatGPT conversations
- **Data migration** planning and analysis

### Development Workflow Integration  
- **Code review** assistance with file system MCP
- **API testing** and documentation with REST MCP
- **Database schema** analysis and optimization

### Business Intelligence
- **Ad-hoc analysis** through natural language queries
- **Report generation** from multiple data sources
- **Trend analysis** combining database and API data

---

> 🔌 **Integration Power:** MCPs transform ChatGPT from a static assistant into a dynamic interface for your entire development ecosystem!