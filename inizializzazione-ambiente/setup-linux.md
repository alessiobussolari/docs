# Setup Linux - Ambiente di Sviluppo

Guida completa per configurare un ambiente di sviluppo Ruby/Rails su distribuzioni Linux basate su Debian/Ubuntu.

## Sistemi Supportati

- **Ubuntu** 20.04 LTS, 22.04 LTS, 24.04 LTS
- **Debian** 11 (Bullseye), 12 (Bookworm)
- **Linux Mint** 20+
- **Pop!_OS** 20.04+

## Prerequisiti

- **Distribuzione Linux** supportata
- **Connessione internet** stabile
- **Privilegi sudo**
- **Terminal access**

## 1. Aggiornamento Sistema

```bash
# Aggiorna il sistema
sudo apt update && sudo apt upgrade -y

# Installa strumenti di base
sudo apt install -y curl wget software-properties-common apt-transport-https ca-certificates gnupg lsb-release
```

## 2. Build Tools Essenziali

```bash
# Installa build tools e librerie di sviluppo
sudo apt install -y build-essential git-core curl libssl-dev libreadline-dev zlib1g-dev \
  libsqlite3-dev libxml2-dev libxslt1-dev libcurl4-openssl-dev \
  libffi-dev libyaml-dev autoconf bison libncurses5-dev

# Librerie aggiuntive per Ruby
sudo apt install -y libgdbm-dev libdb-dev uuid-dev
```

## 3. Version Manager (asdf)

```bash
# Clona asdf
git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.13.1

# Aggiungi al shell profile
echo '. "$HOME/.asdf/asdf.sh"' >> ~/.bashrc
echo '. "$HOME/.asdf/completions/asdf.bash"' >> ~/.bashrc

# Ricarica configurazione
source ~/.bashrc

# Verifica installazione
asdf --version
```

## 4. Ruby Installation

```bash
# Installa plugin Ruby
asdf plugin add ruby https://github.com/asdf-vm/asdf-ruby.git

# Installa dipendenze specifiche Ruby
sudo apt install -y autoconf patch rustc

# Installa Ruby ultima versione
asdf install ruby latest
asdf global ruby latest

# Verifica installazione
ruby --version
gem --version

# Installa Bundler
gem install bundler
```

## 5. Node.js Installation

```bash
# Installa plugin Node.js
asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git

# Installa Node.js
asdf install nodejs latest
asdf global nodejs latest

# Verifica installazione
node --version
npm --version
```

## 6. Database Setup

### PostgreSQL

```bash
# Installa PostgreSQL
sudo apt install -y postgresql postgresql-contrib libpq-dev

# Avvia e abilita servizio
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Configura utente PostgreSQL
sudo -u postgres createuser --superuser $USER
sudo -u postgres psql -c "ALTER USER $USER PASSWORD 'password';"

# Verifica installazione
psql --version
```

### Redis

```bash
# Installa Redis
sudo apt install -y redis-server

# Configura Redis per avvio automatico
sudo systemctl enable redis-server
sudo systemctl start redis-server

# Verifica installazione
redis-cli --version
redis-cli ping
```

## 7. Docker Installation

```bash
# Rimuovi versioni vecchie di Docker
sudo apt remove -y docker docker-engine docker.io containerd runc

# Aggiungi repository Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Installa Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Aggiungi utente al gruppo docker
sudo usermod -aG docker $USER

# Avvia Docker
sudo systemctl enable docker
sudo systemctl start docker

# Installa docker-compose standalone
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

## 8. CLI Utilities

```bash
# HTTPie per API testing
sudo apt install -y httpie

# jq per JSON processing
sudo apt install -y jq

# fzf per fuzzy finding
sudo apt install -y fzf

# bat per syntax highlighting
sudo apt install -y bat
# Su Ubuntu 20.04, potrebbe essere necessario:
# sudo apt install -y batcat && ln -s /usr/bin/batcat ~/.local/bin/bat

# tree per visualizzazione directory
sudo apt install -y tree

# htop per system monitoring
sudo apt install -y htop

# neovim (editor avanzato)
sudo apt install -y neovim

# git-delta per diff migliorati
wget https://github.com/dandavison/delta/releases/download/0.16.5/git-delta_0.16.5_amd64.deb
sudo dpkg -i git-delta_0.16.5_amd64.deb
rm git-delta_0.16.5_amd64.deb
```

## 9. Editor Setup

### VS Code

```bash
# Metodo 1: Via Snap
sudo snap install --classic code

# Metodo 2: Via repository Microsoft
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt update
sudo apt install -y code
```

### Estensioni VS Code Essenziali

```bash
# Ruby/Rails development
code --install-extension shopify.ruby-lsp
code --install-extension bung87.rails
code --install-extension kaiwood.endwise

# Git integration
code --install-extension eamodio.gitlens
code --install-extension mhutchie.git-graph

# General development
code --install-extension ms-vscode.vscode-json
code --install-extension redhat.vscode-yaml
code --install-extension ms-azuretools.vscode-docker
code --install-extension humao.rest-client
code --install-extension rangav.vscode-thunder-client

# Code quality
code --install-extension esbenp.prettier-vscode
code --install-extension ms-vscode.vscode-eslint
code --install-extension bradlc.vscode-tailwindcss
```

## 10. Ruby on Rails Specific Tools

```bash
# ImageMagick per ActiveStorage
sudo apt install -y imagemagick libmagickwand-dev

# Overmind (process manager)
wget https://github.com/DarthSim/overmind/releases/download/v2.4.0/overmind-v2.4.0-linux-amd64.gz
gunzip overmind-v2.4.0-linux-amd64.gz
chmod +x overmind-v2.4.0-linux-amd64
sudo mv overmind-v2.4.0-linux-amd64 /usr/local/bin/overmind

# Installa Rails
gem install rails

# Verifica installazione
rails --version
```

## 11. Git Configuration

```bash
# Configurazione base
git config --global user.name "Il Tuo Nome"
git config --global user.email "tua.email@example.com"
git config --global init.defaultBranch main

# Editor predefinito
git config --global core.editor "code --wait"

# Merge tool
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Configurazione git-delta
git config --global core.pager delta
git config --global interactive.diffFilter "delta --color-only"
git config --global delta.navigate true
git config --global delta.side-by-side true
git config --global merge.conflictstyle diff3
git config --global diff.colorMoved default

# Aliases utili
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.unstage "reset HEAD --"
```

## 12. SSH Setup

```bash
# Genera chiave SSH Ed25519
ssh-keygen -t ed25519 -C "tua.email@example.com"

# Avvia ssh-agent
eval "$(ssh-agent -s)"

# Aggiungi chiave all'agent
ssh-add ~/.ssh/id_ed25519

# Configura SSH agent per avvio automatico
echo 'eval "$(ssh-agent -s)"' >> ~/.bashrc
echo 'ssh-add ~/.ssh/id_ed25519' >> ~/.bashrc

# Mostra chiave pubblica
cat ~/.ssh/id_ed25519.pub
echo "Aggiungi questa chiave al tuo account GitHub/GitLab"
```

## 13. Shell Improvements

### Zsh + Oh My Zsh (Opzionale)

```bash
# Installa Zsh
sudo apt install -y zsh

# Installa Oh My Zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Cambia shell predefinita
chsh -s $(which zsh)

# Installa plugin utili
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

### Alias Utili

Aggiungi al `~/.bashrc` o `~/.zshrc`:

```bash
# Rails aliases
alias be="bundle exec"
alias rc="rails console"
alias rs="rails server"
alias rg="rails generate"
alias rt="rails test"
alias rr="rails routes"

# Git aliases
alias gs="git status"
alias ga="git add"
alias gc="git commit"
alias gp="git push"
alias gl="git pull"
alias gco="git checkout"
alias gcb="git checkout -b"
alias gm="git merge"

# Sistema
alias ll="ls -alF"
alias la="ls -A"
alias l="ls -CF"
alias ..="cd .."
alias ...="cd ../.."
alias grep="grep --color=auto"

# Docker
alias dc="docker-compose"
alias dcu="docker-compose up"
alias dcd="docker-compose down"
alias dcb="docker-compose build"
```

## 14. Environment Variables

```bash
# Crea file per variabili ambiente
echo 'export EDITOR="code --wait"' >> ~/.bashrc
echo 'export RAILS_ENV="development"' >> ~/.bashrc
echo 'export RACK_ENV="development"' >> ~/.bashrc

# Per PostgreSQL
echo 'export PGUSER="$USER"' >> ~/.bashrc
echo 'export PGPASSWORD="password"' >> ~/.bashrc

# Ricarica configurazione
source ~/.bashrc
```

## 15. Verifica Setup Completo

```bash
# Test Ruby environment
ruby --version
gem --version
bundle --version
rails --version

# Test Node.js environment
node --version
npm --version

# Test database
psql --version
redis-cli --version

# Test Docker
docker --version
docker-compose --version

# Test CLI tools
http --version
jq --version
git --version
code --version

# Test ImageMagick
convert -version

# Test Overmind
overmind --version
```

## 16. Performance Optimizations

### Sistema

```bash
# Aumenta limite file aperti
echo '* soft nofile 65536' | sudo tee -a /etc/security/limits.conf
echo '* hard nofile 65536' | sudo tee -a /etc/security/limits.conf

# Ottimizza Git per repository grandi
git config --global core.preloadindex true
git config --global core.fscache true
git config --global gc.auto 256
```

### Ruby Performance

```bash
# Ottimizza Bundler
bundle config set --global jobs 4
bundle config set --global retry 3
bundle config set --global timeout 300

# Configura Bootsnap per Rails
echo 'require "bootsnap/setup"' >> ~/.bashrc
```

## 🆘 Troubleshooting

### Ruby build failures

```bash
# Installa dipendenze mancanti
sudo apt install -y libgdbm-dev libncurses5-dev automake libtool pkg-config libffi-dev

# Clean build Ruby
asdf uninstall ruby [version]
asdf install ruby [version]
```

### PostgreSQL connection issues

```bash
# Riavvia PostgreSQL
sudo systemctl restart postgresql

# Verifica status
sudo systemctl status postgresql

# Controlla log
sudo tail -f /var/log/postgresql/postgresql-*.log
```

### Docker permission denied

```bash
# Assicurati di essere nel gruppo docker
sudo usermod -aG docker $USER

# Riavvia sessione o ricarica gruppi
newgrp docker
```

Per problemi specifici consulta la [guida troubleshooting](troubleshooting.md).
