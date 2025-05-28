# Setup Windows - Ambiente di Sviluppo

Guida completa per configurare un ambiente di sviluppo Ruby/Rails su Windows 10/11 utilizzando WSL2 (Windows Subsystem for Linux).

## Prerequisiti

- **Windows 10** versione 2004+ o **Windows 11**
- **Connessione internet** stabile
- **Privilegi amministratore**
- **WSL2** abilitato

## 1. Abilitazione WSL2

### Installazione WSL2

Apri PowerShell come amministratore ed esegui:

```powershell
# Abilita WSL
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# Abilita Virtual Machine Platform
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# Riavvia il computer
Restart-Computer
```

Dopo il riavvio:

```powershell
# Imposta WSL2 come versione predefinita
wsl --set-default-version 2

# Installa Ubuntu
wsl --install -d Ubuntu-22.04
```

## 2. Setup Ubuntu su WSL2

### Prima Configurazione

```bash
# Aggiorna il sistema
sudo apt update && sudo apt upgrade -y

# Installa curl e build tools essenziali
sudo apt install -y curl git build-essential libssl-dev libreadline-dev zlib1g-dev libsqlite3-dev
```

### Installazione Tools Core

```bash
# Installa asdf (version manager)
git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.13.1
echo '. "$HOME/.asdf/asdf.sh"' >> ~/.bashrc
echo '. "$HOME/.asdf/completions/asdf.bash"' >> ~/.bashrc
source ~/.bashrc

# Installa direnv
sudo apt install -y direnv
echo 'eval "$(direnv hook bash)"' >> ~/.bashrc
```

## 3. Installazione Ruby e Rails

### Ruby via asdf

```bash
# Installa plugin Ruby
asdf plugin add ruby

# Installa dipendenze Ruby
sudo apt install -y autoconf bison build-essential libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev

# Installa Ruby ultima versione
asdf install ruby latest
asdf global ruby latest

# Verifica installazione
ruby --version
gem --version
```

### Node.js via asdf

```bash
# Installa plugin Node.js
asdf plugin add nodejs

# Installa Node.js
asdf install nodejs latest
asdf global nodejs latest

# Verifica installazione
node --version
npm --version
```

## 4. Database Setup

### PostgreSQL

```bash
# Installa PostgreSQL
sudo apt install -y postgresql postgresql-contrib libpq-dev

# Avvia servizio
sudo service postgresql start

# Configura utente
sudo -u postgres createuser --superuser $USER
sudo -u postgres psql -c "ALTER USER $USER PASSWORD 'password';"
```

### Redis

```bash
# Installa Redis
sudo apt install -y redis-server

# Avvia servizio
sudo service redis-server start
```

## 5. CLI Utilities

```bash
# HTTPie per API testing
sudo apt install -y httpie

# jq per JSON processing
sudo apt install -y jq

# fzf per fuzzy finding
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install

# bat (sostituto cat con syntax highlighting)
sudo apt install -y bat

# tree per visualizzazione directory
sudo apt install -y tree

# htop per monitoring processi
sudo apt install -y htop

# git-delta per diff migliorati
wget https://github.com/dandavison/delta/releases/download/0.16.5/git-delta_0.16.5_amd64.deb
sudo dpkg -i git-delta_0.16.5_amd64.deb
```

## 6. Editor Setup

### VS Code con WSL Extension

1. Installa **VS Code** su Windows
2. Installa l'estensione **Remote - WSL**
3. Da WSL, apri progetti con: `code .`

### Estensioni VS Code Consigliate

Installa tramite comando palette (Ctrl+Shift+P):

```bash
# Ruby/Rails extensions
code --install-extension shopify.ruby-lsp
code --install-extension bung87.rails
code --install-extension eamodio.gitlens

# General development
code --install-extension ms-vscode.vscode-json
code --install-extension redhat.vscode-yaml
code --install-extension ms-azuretools.vscode-docker
code --install-extension humao.rest-client
```

## 7. Docker Setup

### Docker Desktop per Windows

1. Scarica **Docker Desktop** dal sito ufficiale
2. Installa con supporto WSL2
3. Nelle impostazioni, abilita **WSL2 integration**

Verifica installazione da WSL:

```bash
docker --version
docker-compose --version
```

## 8. Git Configuration

```bash
# Configurazione base
git config --global user.name "Il Tuo Nome"
git config --global user.email "tua.email@example.com"

# Configurazione avanzata
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'

# Setup delta per diff migliorati
git config --global core.pager delta
git config --global interactive.diffFilter "delta --color-only"
git config --global delta.navigate true
git config --global merge.conflictstyle diff3
git config --global diff.colorMoved default
```

## 9. SSH Setup

```bash
# Genera chiave SSH
ssh-keygen -t ed25519 -C "tua.email@example.com"

# Avvia ssh-agent
eval "$(ssh-agent -s)"

# Aggiungi chiave
ssh-add ~/.ssh/id_ed25519

# Mostra chiave pubblica per aggiungerla a GitHub/GitLab
cat ~/.ssh/id_ed25519.pub
```

## 10. Shell Improvements

### Zsh (Opzionale)

```bash
# Installa Zsh
sudo apt install -y zsh

# Installa Oh My Zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Configura come shell predefinita
chsh -s $(which zsh)
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

# Git aliases
alias gs="git status"
alias ga="git add"
alias gc="git commit"
alias gp="git push"
alias gl="git pull"
alias gco="git checkout"

# Sistema
alias ll="ls -la"
alias la="ls -A"
alias l="ls -CF"
```

## 11. Rails New Project

```bash
# Installa Rails
gem install rails

# Crea nuovo progetto
rails new myapp --api --database=postgresql --skip-action-text --skip-action-mailer --skip-action-mailbox --skip-active-storage --skip-action-cable --skip-javascript --skip-hotwire --skip-jbuilder

cd myapp

# Setup database
rails db:create
rails db:migrate
```

## 12. Verifica Setup Completo

```bash
# Verifica Ruby
ruby --version

# Verifica Rails
rails --version

# Verifica Node.js
node --version

# Verifica Database
psql --version
redis-cli --version

# Verifica Docker
docker --version

# Verifica Git
git --version

# Verifica CLI tools
http --version
jq --version
fzf --version
bat --version
tree --version
```

## 🔧 Performance Tips

### WSL2 Optimizations

```bash
# Aggiungi al ~/.bashrc per performance migliori
echo 'export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk "{print \$2}"):0' >> ~/.bashrc
echo 'export LIBGL_ALWAYS_INDIRECT=1' >> ~/.bashrc
```

### Git Performance

```bash
# Miglioramenti performance Git su WSL
git config --global core.preloadindex true
git config --global core.fscache true
git config --global gc.auto 256
```

## 🆘 Troubleshooting

### WSL2 non funziona

```powershell
# Verifica versione WSL
wsl --list --verbose

# Aggiorna WSL
wsl --update
```

### Problemi di connettività

```bash
# Reset DNS WSL
sudo rm /etc/resolv.conf
sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
sudo bash -c 'echo "[network]" > /etc/wsl.conf'
sudo bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
```

### PostgreSQL non si avvia

```bash
# Riavvia PostgreSQL
sudo service postgresql restart

# Verifica status
sudo service postgresql status
```

Per problemi specifici consulta la [guida troubleshooting](troubleshooting.md).
