# 🍎 macOS Development Setup

Configuração completa e otimização do macOS para desenvolvimento, com foco em produtividade e ferramentas específicas.

## 🍺 **Package Management**

- **[Homebrew Complete Setup](homebrew/homebrew.md)** - Lista curada de aplicações e ferramentas essenciais

## ⚙️ **System Configuration**

### Essential System Tweaks
```bash
# Mostrar ficheiros ocultos
defaults write com.apple.finder AppleShowAllFiles -bool true

# Acelerar animações do Dock
defaults write com.apple.dock autohide-time-modifier -int 0
defaults write com.apple.dock autohide-delay -float 0

# Desativar gatekeeper para desenvolvimento
sudo spctl --master-disable
```

### Development-Focused Settings
- **Spotlight indexing** otimizado para projetos
- **Finder** configurado para desenvolvedores
- **Dock** minimalista para mais espaço de ecrã
- **Keyboard shortcuts** personalizados

## 🛠️ **Development Tools**

### Command Line Essentials
```bash
# Instalar Command Line Tools
xcode-select --install

# Homebrew (se ainda não estiver instalado)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Essential development tools
brew install git node python uv fnm
```

### macOS-Specific Tools
- **Rectangle** - Window management
- **Alfred** - Productivity launcher  
- **iTerm2** - Terminal replacement
- **Raycast** - Modern launcher alternative

## 📱 **Productivity Apps**

### Code Editors & IDEs
- **VSCode** com extensions para desenvolvimento
- **Cursor** - AI-powered code editor
- **WebStorm** para JavaScript/TypeScript
- **PyCharm** para Python development

### Development Utilities
- **TablePlus** - Database management
- **Postman** - API development
- **Docker Desktop** - Containerization
- **GitHub Desktop** - Git GUI

## 🔒 **Security & Privacy**

### Development Security
- **SSH key management** com Keychain
- **GPG signing** para commits
- **1Password** para secrets management
- **Little Snitch** para network monitoring

### System Security
```bash
# Configurar firewall
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on

# FileVault encryption
sudo fdesetup enable
```

## 🎨 **UI/UX Design Tools**

- **Figma** - Design collaboration
- **Sketch** - macOS native design
- **ImageOptim** - Image optimization
- **CleanMyMac** - System maintenance

## 🚀 **Performance Optimization**

### System Maintenance
```bash
# Limpar cache do sistema
sudo purge

# Reconstruir Spotlight index
sudo mdutil -E /

# Verificar uso de armazenamento
sudo du -sh /Users/*/Library
```

### Development Performance
- **Disable indexing** em diretórios de build
- **SSD optimization** para performance
- **Memory management** para projetos grandes
- **Background app management**

---

> 🍎 **macOS Tip:** Usa Homebrew Bundle (`brew bundle`) para manter as tuas ferramentas sincronizadas entre máquinas!