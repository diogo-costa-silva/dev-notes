# 🍎 macOS Development Setup

Configuração completa e otimização do macOS para desenvolvimento, com foco em produtividade e ferramentas específicas.

## 🍺 **Package Management**

### Homebrew - The Essential Package Manager
```bash
# Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Add to PATH (Apple Silicon)
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"

# Verify installation
brew --version && brew doctor
```

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

### Essential Development Apps
```bash
# Terminals and editors
brew install --cask iterm2
brew install --cask visual-studio-code

# Development utilities
brew install --cask docker
brew install --cask dbeaver-community
brew install android-platform-tools

# Web browsers
brew install --cask arc
brew install --cask google-chrome
```

### Productivity & Communication
```bash
# AI and communication
brew install --cask chatgpt
brew install --cask deepl
brew install --cask notion

# File management
brew install --cask google-drive
brew install --cask macdroid
brew install --cask qbittorrent

# Security
brew install --cask keepassxc
```

## 📱 **Accessibility & System Tools**

### Mac App Store Apps
```bash
# System utilities
mas install 937984704   # Amphetamine - Keep Mac awake
mas install 441258766   # Magnet - Window management

# Productivity
mas install 1295203466  # Microsoft Remote Desktop
mas install 1289583905  # Pixelmator Pro
```

### System Enhancement
```bash
# Scroll wheel control
brew install --cask unnaturalscrollwheels

# System monitoring
brew install htop btop
brew install --cask stats
```

## 🔒 **Security & Privacy**

### Development Security
```bash
# Configure Git with SSH
ssh-keygen -t ed25519 -C "your.email@example.com"
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519

# Configure Git
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global core.editor "code --wait"
```

### System Security
```bash
# Enable firewall
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on

# FileVault encryption (if not enabled)
sudo fdesetup enable

# Show security status
sudo fdesetup status
```

## 📝 **Shell & Terminal Enhancement**

### Modern Shell Tools
```bash
# Enhanced command line tools
brew install starship     # Shell prompt
brew install bat          # Better cat
brew install exa          # Better ls
brew install ripgrep      # Better grep
brew install fd           # Better find
brew install tree         # Directory tree
```

### Shell Configuration
```bash
# Add to ~/.zshrc
cat >> ~/.zshrc << 'EOF'
# Homebrew
export PATH="/opt/homebrew/bin:$PATH"

# Development tools
export PATH="$HOME/.local/bin:$PATH"

# Aliases
alias ll='exa -la'
alias cat='bat'
alias find='fd'

# Git shortcuts
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'

# Shell prompt
eval "$(starship init zsh)"
EOF
```

## 🚀 **Complete Setup Automation**

### One-Click Setup Script
```bash
#!/bin/bash
# save as: setup-mac-dev.sh

echo "🚀 Starting macOS Development Setup..."

# Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Essential CLI tools
brew install git gh node python uv tree htop bat exa ripgrep fd starship

# Development applications
brew install --cask visual-studio-code iterm2 docker arc

# Communication and AI
brew install --cask chatgpt deepl notion

# Utilities
brew install --cask keepassxc google-drive macdroid
brew install --cask unnaturalscrollwheels

# Mac App Store apps
brew install mas
mas install 937984704  # Amphetamine

# Configure shell
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
echo 'alias ll="exa -la"' >> ~/.zshrc
echo 'alias cat="bat"' >> ~/.zshrc

echo "✅ macOS Development Setup Complete!"
```

### Run Setup
```bash
chmod +x setup-mac-dev.sh
./setup-mac-dev.sh
```

## 🔄 **Maintenance**

### Regular Updates
```bash
# Update all Homebrew packages
brew update && brew upgrade && brew cleanup

# Update Mac App Store apps
mas upgrade

# System software updates
sudo softwareupdate -i -a
```

### Backup Important Configs
```bash
# Essential files to backup
~/.zshrc
~/.gitconfig
~/Library/Application Support/Code/User/settings.json
```

---

> 🍎 **Pro Tip:** Use the automation script to set up a new Mac in minutes, then customize based on your specific needs!