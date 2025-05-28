# Setup macOS - Ambiente di Sviluppo

Setup automatico completo per ambiente di sviluppo moderno su macOS, ottimizzato per sviluppo Ruby/Rails con tutti gli strumenti essenziali e opzioni di personalizzazione avanzate.

## Prerequisiti

- **macOS** versione supportata (10.15+)
- **Connessione internet** stabile
- **Privilegi amministratore** per installazioni system-wide
- **Command Line Tools** per Xcode (installati automaticamente se necessario)

## Setup Automatico (Raccomandato)

### Quick Start

```bash
# Clona il repository
git clone https://bitbucket.org/pandev-srl/setup-dev-mac.git
cd setup-dev-mac

# Rendi eseguibile lo script
chmod +x setup.sh

# Esegui il setup completo
./setup.sh
```

### Cosa Include lo Script

Lo script di setup automatico installa e configura:

## ✨ Caratteristiche Principali

### 🛠️ Tools Opzionali Personalizzabili
- **25+ CLI tools** aggiuntivi selezionabili
- **iTerm2 Status Bar** con metriche sistema in tempo reale
- **VS Code** preconfigurato con 13 estensioni essenziali
- **SSH Setup** automatico con chiavi Ed25519
- **Dotfiles** essenziali preconfigurati
- **macOS** ottimizzazioni per sviluppo
- **Configurazione zero** - tutto automatico e personalizzabile

## 📦 Strumenti Installati

### Core Development (Automatici)
- **Homebrew** - Package manager per macOS
- **iTerm2** - Terminal avanzato per sviluppatori
- **Git** - Versione moderna via Homebrew + git-completion avanzato
- **asdf** - Version manager per Python, Node.js, Ruby
- **direnv** - Gestione environment variables per progetto
- **autojump** - Navigazione veloce directory

### Ruby on Rails Specifico
- **ImageMagick** - Processing immagini per ActiveStorage
- **libpq** - Header PostgreSQL per gem pg
- **Overmind** - Gestione processi multipli (superiore a Foreman)

### CLI Utilities Essenziali
- **HTTPie** - Tool HTTP user-friendly per API testing
- **jq** - Parser JSON per CLI
- **git-delta** - Diff colorati e migliorati per Git
- **fzf** - Fuzzy finder per ricerca veloce
- **bat** - Sostituto di cat con syntax highlighting
- **tree** - Visualizzazione directory ad albero
- **htop** - Monitor processi avanzato

### Containerizzazione
- **Docker Desktop** - Containerizzazione
- **docker-compose** - Orchestrazione container
- **lazydocker** - TUI Docker per gestione container

## 🔧 Configurazioni Avanzate

### iTerm2 Status Bar
Lo script configura automaticamente una status bar con:
- **CPU Usage** - Utilizzo processore in tempo reale
- **Memory Usage** - Memoria RAM utilizzata
- **Network Throughput** - Traffico rete
- **Git Branch** - Branch corrente quando in repository

### VS Code Extensions
Estensioni installate automaticamente:
- **Ruby LSP** - Language server per Ruby
- **Rails** - Supporto framework Rails
- **GitLens** - Git supercharged
- **Bracket Pair Colorizer** - Colorazione parentesi
- **Auto Rename Tag** - Rename automatico tag HTML
- **Prettier** - Code formatter
- **ESLint** - Linting JavaScript
- **Docker** - Supporto container
- **YAML** - Supporto file YAML
- **Markdown** - Preview e editing markdown
- **REST Client** - Testing API direttamente da VS Code
- **Thunder Client** - Client API completo
- **Ruby Solargraph** - IntelliSense Ruby

### SSH Setup Automatico
- Generazione **chiave Ed25519** sicura
- Configurazione **SSH agent**
- Setup **config SSH** ottimizzato

### Dotfiles Essenziali
- **.gitconfig** - Configurazione Git avanzata
- **.gitignore_global** - Ignore patterns globali
- **.zshrc** - Configurazione shell Zsh
- **.asdfrc** - Configurazione asdf

## 🍎 Ottimizzazioni macOS

### System Preferences
Lo script ottimizza:
- **Configurazione zero** - nessuna interazione manuale richiesta
- **Performance** - Disabilitazione animazioni superflue
- **Developer Tools** - Configurazioni per sviluppo
- **Security** - Impostazioni sicurezza bilanciate

## 📋 Setup Manuale (Alternativo)

Se preferisci un setup step-by-step:

### 1. Homebrew
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. Core Tools
```bash
brew install git asdf direnv autojump
```

### 3. iTerm2
```bash
brew install --cask iterm2
```

### 4. VS Code
```bash
brew install --cask visual-studio-code
```

### 5. Ruby Tools
```bash
brew install imagemagick libpq overmind
```

### 6. CLI Utilities
```bash
brew install httpie jq git-delta fzf bat tree htop
```

### 7. Docker
```bash
brew install --cask docker
brew install lazydocker
```

## 🔍 Verifica Installazione

Dopo il setup, verifica che tutto sia installato correttamente:

```bash
# Verifica Homebrew
brew --version

# Verifica Git
git --version

# Verifica asdf
asdf --version

# Verifica Ruby tools
convert -version  # ImageMagick
overmind --version

# Verifica CLI tools
http --version    # HTTPie
jq --version
delta --version   # git-delta
fzf --version
bat --version
tree --version

# Verifica Docker
docker --version
docker-compose --version
```

## 🚀 Post-Setup

### Configurazione Ruby
```bash
# Installa Ruby via asdf
asdf plugin add ruby
asdf install ruby latest
asdf global ruby latest

# Verifica installazione
ruby --version
gem --version
```

### Configurazione Node.js
```bash
# Installa Node.js via asdf
asdf plugin add nodejs
asdf install nodejs latest
asdf global nodejs latest

# Verifica installazione
node --version
npm --version
```

### Configurazione Python
```bash
# Installa Python via asdf
asdf plugin add python
asdf install python latest
asdf global python latest

# Verifica installazione
python --version
pip --version
```

## 🔧 Personalizzazioni

### iTerm2 Themes
Puoi personalizzare il tema iTerm2:
```bash
# Scarica temi popolari
curl -Ls https://raw.githubusercontent.com/mbadolato/iTerm2-Color-Schemes/master/schemes/Dracula.itermcolors > ~/Downloads/Dracula.itermcolors
```

### Git Configuration
Personalizza la configurazione Git:
```bash
git config --global user.name "Il Tuo Nome"
git config --global user.email "tua.email@example.com"
```

### Shell Customization
Aggiungi alias utili al `.zshrc`:
```bash
echo 'alias ll="ls -la"' >> ~/.zshrc
echo 'alias gs="git status"' >> ~/.zshrc
echo 'alias gp="git push"' >> ~/.zshrc
```

## 🆘 Troubleshooting

### Problemi Comuni

**Homebrew non trovato:**
```bash
echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Permessi negati:**
```bash
sudo chown -R $(whoami) /opt/homebrew/share/zsh /opt/homebrew/share/zsh/site-functions
```

**asdf non funziona:**
```bash
echo '. $(brew --prefix asdf)/libexec/asdf.sh' >> ~/.zshrc
source ~/.zshrc
```

Per problemi specifici consulta la [guida troubleshooting](troubleshooting.md).
