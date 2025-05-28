# Troubleshooting - Ambiente di Sviluppo

Soluzioni ai problemi più comuni durante la configurazione dell'ambiente di sviluppo.

## 🍎 macOS Specific Issues

### Homebrew Issues

**Problema: Homebrew non trovato dopo installazione**
```bash
# Soluzione: Aggiungi al PATH
echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# Per macOS Intel
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zshrc
```

**Problema: Permessi negati su Homebrew**
```bash
# Fix permessi Homebrew
sudo chown -R $(whoami) /opt/homebrew/share/zsh /opt/homebrew/share/zsh/site-functions

# Per macOS Intel
sudo chown -R $(whoami) /usr/local/share/zsh /usr/local/share/zsh/site-functions
```

**Problema: Xcode Command Line Tools mancanti**
```bash
# Installa manualmente
xcode-select --install

# Se fallisce, scarica manualmente da Apple Developer
```

### asdf Issues

**Problema: asdf non funziona**
```bash
# Aggiungi al shell profile
echo '. $(brew --prefix asdf)/libexec/asdf.sh' >> ~/.zshrc
echo '. $(brew --prefix asdf)/etc/bash_completion.d/asdf.bash' >> ~/.zshrc
source ~/.zshrc
```

**Problema: Ruby build failure su macOS**
```bash
# Installa dipendenze mancanti
brew install openssl readline libyaml

# Export variabili per Ruby build
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)"
asdf install ruby [version]
```

### VS Code Issues

**Problema: code command non trovato**
```bash
# Apri VS Code manualmente
# Cmd+Shift+P -> "Shell Command: Install 'code' command in PATH"

# O installa manualmente
sudo ln -fs "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code" /usr/local/bin/code
```

## 🪟 Windows/WSL Issues

### WSL2 Setup Issues

**Problema: WSL2 non si avvia**
```powershell
# Verifica che WSL2 sia abilitato
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

# Riavvia e aggiorna WSL
wsl --update
wsl --set-default-version 2
```

**Problema: Virtualizzazione non supportata**
```powershell
# Verifica supporto virtualizzazione nel BIOS
systeminfo

# Abilita Hyper-V se necessario
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

### Network Issues in WSL

**Problema: DNS non funziona in WSL**
```bash
# Fix DNS resolution
sudo rm /etc/resolv.conf
sudo bash -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
sudo bash -c 'echo "nameserver 8.8.4.4" >> /etc/resolv.conf'

# Previeni sovrascrittura automatica
sudo bash -c 'echo "[network]" > /etc/wsl.conf'
sudo bash -c 'echo "generateResolvConf = false" >> /etc/wsl.conf'
```

**Problema: Port forwarding non funziona**
```powershell
# Windows: Forward porta WSL
netsh interface portproxy add v4tov4 listenport=3000 listenaddress=0.0.0.0 connectport=3000 connectaddress=172.x.x.x

# Trova IP WSL
wsl hostname -I
```

### File System Performance

**Problema: Lentezza file system**
```bash
# Lavora sempre nel file system Linux (/home/user)
# Evita /mnt/c/ per progetti attivi

# Migliora performance Git
git config --global core.autocrlf false
git config --global core.filemode false
```

## 🐧 Linux Issues

### Package Manager Issues

**Problema: Repository non trovati**
```bash
# Ubuntu: Aggiorna repository
sudo apt update && sudo apt upgrade

# Se fallisce, reset sources.list
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
sudo sed -i 's/archive.ubuntu.com/mirror.ubuntu.com/g' /etc/apt/sources.list
```

**Problema: GPG key errors**
```bash
# Fix chiavi GPG mancanti
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys [KEY_ID]

# O reset completamente
sudo rm -rf /var/lib/apt/lists/*
sudo apt update
```

### Permission Issues

**Problema: Docker permission denied**
```bash
# Aggiungi utente al gruppo docker
sudo usermod -aG docker $USER

# Ricarica gruppi senza logout
newgrp docker

# Testa
docker run hello-world
```

**Problema: npm global permission issues**
```bash
# Configura npm per non usare sudo
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

## 🗄️ Database Issues

### PostgreSQL Issues

**Problema: PostgreSQL non si avvia**
```bash
# Ubuntu/Debian
sudo systemctl start postgresql
sudo systemctl enable postgresql

# macOS
brew services start postgresql

# Verifica status
sudo systemctl status postgresql
```

**Problema: Authentication failed**
```bash
# Reset password PostgreSQL
sudo -u postgres psql
postgres=# ALTER USER postgres PASSWORD 'newpassword';
postgres=# \q

# Configura peer authentication
sudo nano /etc/postgresql/*/main/pg_hba.conf
# Cambia "peer" in "md5" per local connections
sudo systemctl restart postgresql
```

**Problema: Database creation fails**
```bash
# Crea utente se non esiste
sudo -u postgres createuser --superuser $USER

# Se fallisce, usa psql
sudo -u postgres psql -c "CREATE USER $USER WITH SUPERUSER;"
```

### Redis Issues

**Problema: Redis non raggiungibile**
```bash
# Avvia Redis
sudo systemctl start redis-server
sudo systemctl enable redis-server

# macOS
brew services start redis

# Test connessione
redis-cli ping
```

**Problema: Redis bind error**
```bash
# Modifica configurazione
sudo nano /etc/redis/redis.conf

# Trova e modifica:
# bind 127.0.0.1 ::1
# port 6379

sudo systemctl restart redis-server
```

## 💎 Ruby/Rails Issues

### Ruby Installation Issues

**Problema: Ruby build failure**
```bash
# Ubuntu: Installa dipendenze
sudo apt install -y autoconf bison build-essential libssl-dev libyaml-dev \
libreadline6-dev zlib1g-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev

# macOS: Usa Homebrew versions
brew install ruby-build
asdf install ruby latest
```

**Problema: Native extension compilation errors**
```bash
# Installa development headers
sudo apt install -y ruby-dev

# macOS: Assicurati che Xcode tools siano aggiornati
xcode-select --install

# Force reinstall problematic gems
gem install [gem_name] -- --use-system-libraries
```

### Rails Issues

**Problema: Rails server non si avvia**
```bash
# Verifica porta disponibile
lsof -i :3000

# Kill processo se necessario
kill -9 $(lsof -ti:3000)

# Avvia su porta diversa
rails server -p 3001
```

**Problema: Database connection error**
```bash
# Verifica database.yml
cat config/database.yml

# Crea database se non esiste
rails db:create

# Reset database
rails db:drop db:create db:migrate
```

**Problema: Asset compilation failures**
```bash
# Clear assets cache
rails assets:clobber

# Precompile assets
rails assets:precompile

# Check Node.js version compatibility
node --version
```

## 🐳 Docker Issues

### Docker Installation Issues

**Problema: Docker daemon not running**
```bash
# Linux: Avvia Docker
sudo systemctl start docker
sudo systemctl enable docker

# macOS: Assicurati Docker Desktop sia in esecuzione
open /Applications/Docker.app
```

**Problema: Docker permission denied**
```bash
# Aggiungi utente al gruppo docker
sudo usermod -aG docker $USER

# Logout/login o:
newgrp docker
```

### Docker Compose Issues

**Problema: docker-compose not found**
```bash
# Installa docker-compose
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Verifica
docker-compose --version
```

**Problema: Port already in use**
```bash
# Trova processo usando la porta
lsof -i :3000

# Cambia porta in docker-compose.yml
ports:
  - "3001:3000"
```

## 🌐 Network & Connectivity Issues

### Git/GitHub Issues

**Problema: SSH key authentication fails**
```bash
# Testa connessione SSH
ssh -T git@github.com

# Genera nuova chiave se necessario
ssh-keygen -t ed25519 -C "your.email@example.com"
cat ~/.ssh/id_ed25519.pub

# Aggiungi a ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

**Problema: HTTPS authentication issues**
```bash
# Use token per authentication
git config --global credential.helper store
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

# Clone con token
git clone https://[token]@github.com/user/repo.git
```

### SSL/TLS Issues

**Problema: SSL certificate verification failed**
```bash
# Ubuntu: Aggiorna certificati
sudo apt update && sudo apt install ca-certificates

# Fix Git SSL
git config --global http.sslVerify false  # Temporaneo!

# Soluzione permanente: aggiorna certificati
sudo update-ca-certificates
```

## 🔧 Performance Issues

### Slow Terminal/Shell

**Problema: Shell lento all'avvio**
```bash
# Profila shell startup
time zsh -i -c exit

# Commenta temporaneamente plugins in .zshrc
# Identifica plugin problematici

# Ottimizza PATH
echo $PATH | tr ':' '\n' | nl  # Verifica duplicati
```

### Slow Git Operations

**Problema: Git operazioni lente**
```bash
# Ottimizza Git
git config --global core.preloadindex true
git config --global core.fscache true
git config --global gc.auto 256

# Per repository grandi
git config --global core.untrackedCache true
```

### Memory Issues

**Problema: Sistema lento/out of memory**
```bash
# Verifica memoria
free -h

# Trova processi memory-intensive
ps aux --sort=-%mem | head -10

# Configura swap se necessario
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

## 📝 Environment Variables Issues

**Problema: Variables non persistenti**
```bash
# Aggiungi a shell profile corretto
echo 'export VARIABLE_NAME="value"' >> ~/.bashrc  # Bash
echo 'export VARIABLE_NAME="value"' >> ~/.zshrc   # Zsh

# Ricarica
source ~/.bashrc  # o ~/.zshrc
```

**Problema: PATH conflicts**
```bash
# Verifica PATH attuale
echo $PATH | tr ':' '\n' | nl

# Clean duplicati in shell profile
# Metti export più specifici dopo quelli generali
```

## 🆘 Emergency Recovery

### Reset Completo Ambiente

```bash
#!/bin/bash
# reset_dev_env.sh - USA CON CAUTELA!

echo "⚠️  ATTENZIONE: Questo resetterà l'ambiente di sviluppo"
read -p "Continuare? (y/N): " -n 1 -r
echo
if [[ ! $REPLY =~ ^[Yy]$ ]]; then
    exit 1
fi

# Backup configurazioni esistenti
mkdir -p ~/.config_backup/$(date +%Y%m%d)
cp ~/.bashrc ~/.zshrc ~/.gitconfig ~/.ssh/config ~/.config_backup/$(date +%Y%m%d)/ 2>/dev/null

# Reset asdf
rm -rf ~/.asdf

# Reset Homebrew (macOS)
if [[ "$OSTYPE" == "darwin"* ]]; then
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/uninstall.sh)"
fi

# Reset configurazioni shell
cp ~/.bashrc.bak ~/.bashrc 2>/dev/null || echo '# Reset bashrc' > ~/.bashrc
cp ~/.zshrc.bak ~/.zshrc 2>/dev/null || echo '# Reset zshrc' > ~/.zshrc

echo "✅ Reset completato. Riavvia il terminale e ri-esegui il setup."
```

## 📞 Getting Help

Quando tutti gli altri tentativi falliscono:

1. **Verifica la documentazione ufficiale** dello strumento specifico
2. **Cerca l'errore esatto** su Google/Stack Overflow
3. **Controlla i log** del sistema:
   ```bash
   # Linux system logs
   sudo journalctl -xe
   
   # macOS system logs
   log show --predicate 'process CONTAINS "error"' --last 1h
   ```
4. **Crea un ambiente di test isolato** (Docker/VM)
5. **Chiedi aiuto al team** con informazioni dettagliate:
   - Sistema operativo e versione
   - Comando esatto che fallisce
   - Messaggio di errore completo
   - Steps per riprodurre il problema

## 🔍 Debug Tools

### System Information

```bash
# Sistema
uname -a
lsb_release -a  # Linux
sw_vers        # macOS

# Risorse
df -h          # Disk space
free -h        # Memory (Linux)
vm_stat        # Memory (macOS)

# Network
ping google.com
nslookup github.com
```

### Development Environment Debug

```bash
# Ruby environment
gem env
bundle env

# Node environment
npm config list
node -p "process.versions"

# Git environment
git config --list --show-origin
```

Usa questi strumenti per diagnosticare problemi e fornire informazioni utili quando cerchi aiuto.
