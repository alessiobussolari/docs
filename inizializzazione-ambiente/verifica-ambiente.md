# Verifica Ambiente di Sviluppo

Script e controlli per verificare che l'ambiente di sviluppo sia configurato correttamente dopo l'installazione.

## Script di Verifica Automatico

Salva questo script come `check_env.sh` e rendilo eseguibile:

```bash
#!/bin/bash

# Colori per output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Funzioni helper
print_header() {
    echo -e "\n${BLUE}=== $1 ===${NC}"
}

check_command() {
    if command -v "$1" &> /dev/null; then
        echo -e "${GREEN}✓${NC} $1 installato: $(command -v "$1")"
        if [ "$2" ]; then
            echo -e "  Versione: $($1 $2 2>/dev/null || echo 'N/A')"
        fi
        return 0
    else
        echo -e "${RED}✗${NC} $1 non trovato"
        return 1
    fi
}

check_service() {
    if systemctl is-active --quiet "$1" 2>/dev/null; then
        echo -e "${GREEN}✓${NC} Servizio $1 attivo"
        return 0
    elif pgrep -x "$1" > /dev/null; then
        echo -e "${GREEN}✓${NC} Processo $1 in esecuzione"
        return 0
    else
        echo -e "${RED}✗${NC} Servizio $1 non attivo"
        return 1
    fi
}

# Inizio verifiche
echo -e "${BLUE}🔍 Verifica Ambiente di Sviluppo PANDEV${NC}"
echo "Data: $(date)"
echo "Sistema: $(uname -s) $(uname -r)"
echo "Utente: $(whoami)"

# 1. Strumenti Core
print_header "Strumenti Core"
check_command "git" "--version"
check_command "curl" "--version"
check_command "wget" "--version"

# 2. Version Managers
print_header "Version Managers"
check_command "asdf" "--version"
if command -v asdf &> /dev/null; then
    echo "Plugin asdf installati:"
    asdf plugin list 2>/dev/null | sed 's/^/  - /'
fi

# 3. Linguaggi di Programmazione
print_header "Linguaggi di Programmazione"
check_command "ruby" "--version"
check_command "gem" "--version"
check_command "bundle" "--version"
check_command "rails" "--version"
check_command "node" "--version"
check_command "npm" "--version"

# 4. Database
print_header "Database"
check_command "psql" "--version"
check_command "redis-cli" "--version"

if command -v psql &> /dev/null; then
    if pg_isready -q 2>/dev/null; then
        echo -e "${GREEN}✓${NC} PostgreSQL server disponibile"
    else
        echo -e "${YELLOW}⚠${NC} PostgreSQL server non raggiungibile"
    fi
fi

if command -v redis-cli &> /dev/null; then
    if redis-cli ping &> /dev/null; then
        echo -e "${GREEN}✓${NC} Redis server disponibile"
    else
        echo -e "${YELLOW}⚠${NC} Redis server non raggiungibile"
    fi
fi

# 5. Container
print_header "Containerizzazione"
check_command "docker" "--version"
check_command "docker-compose" "--version"

if command -v docker &> /dev/null; then
    if docker info &> /dev/null; then
        echo -e "${GREEN}✓${NC} Docker daemon in esecuzione"
    else
        echo -e "${RED}✗${NC} Docker daemon non in esecuzione"
    fi
fi

# 6. Editor
print_header "Editor e IDE"
check_command "code" "--version"
check_command "vim" "--version"
check_command "nvim" "--version"

# 7. CLI Utilities
print_header "CLI Utilities"
check_command "http" "--version"
check_command "jq" "--version"
check_command "fzf" "--version"
check_command "bat" "--version"
check_command "tree" "--version"
check_command "htop" "--version"
check_command "delta" "--version"

# 8. Rails Specific
print_header "Rails Specific Tools"
check_command "overmind" "--version"
check_command "convert" "-version" # ImageMagick

# 9. SSH
print_header "SSH Configuration"
if [ -f ~/.ssh/id_ed25519 ]; then
    echo -e "${GREEN}✓${NC} Chiave SSH Ed25519 presente"
    echo "  Fingerprint: $(ssh-keygen -lf ~/.ssh/id_ed25519.pub 2>/dev/null | awk '{print $2}')"
elif [ -f ~/.ssh/id_rsa ]; then
    echo -e "${YELLOW}⚠${NC} Chiave SSH RSA presente (consigliata Ed25519)"
else
    echo -e "${RED}✗${NC} Nessuna chiave SSH trovata"
fi

# 10. Git Configuration
print_header "Git Configuration"
GIT_USER=$(git config --global user.name 2>/dev/null)
GIT_EMAIL=$(git config --global user.email 2>/dev/null)

if [ "$GIT_USER" ]; then
    echo -e "${GREEN}✓${NC} Git user.name: $GIT_USER"
else
    echo -e "${RED}✗${NC} Git user.name non configurato"
fi

if [ "$GIT_EMAIL" ]; then
    echo -e "${GREEN}✓${NC} Git user.email: $GIT_EMAIL"
else
    echo -e "${RED}✗${NC} Git user.email non configurato"
fi

# 11. Environment Variables
print_header "Environment Variables"
ENV_VARS=("EDITOR" "SHELL" "PATH")
for var in "${ENV_VARS[@]}"; do
    if [ "${!var}" ]; then
        echo -e "${GREEN}✓${NC} $var: ${!var}"
    else
        echo -e "${YELLOW}⚠${NC} $var non impostato"
    fi
done

# 12. Network Connectivity
print_header "Connettività"
if ping -c 1 google.com &> /dev/null; then
    echo -e "${GREEN}✓${NC} Connessione internet disponibile"
else
    echo -e "${RED}✗${NC} Problemi di connettività"
fi

if command -v git &> /dev/null; then
    if git ls-remote https://github.com/rails/rails.git &> /dev/null; then
        echo -e "${GREEN}✓${NC} Accesso a GitHub disponibile"
    else
        echo -e "${RED}✗${NC} Problemi di accesso a GitHub"
    fi
fi

# Summary
print_header "Riepilogo"
echo "Verifica completata. Controlla i punti contrassegnati con ✗ o ⚠ sopra."
echo -e "\nPer risolvere problemi specifici, consulta: ${BLUE}troubleshooting.md${NC}"
```

## Verifica Rapida Manuale

### 1. Test Ruby Environment

```bash
# Verifica Ruby
ruby --version
gem --version
bundle --version

# Test Gem installation
gem install colorize
ruby -e "require 'colorize'; puts 'Ruby OK'.green"
gem uninstall colorize

# Verifica Rails
rails --version
rails new test_app --skip-git --skip-bundle --api
cd test_app && rails generate model Test name:string
cd .. && rm -rf test_app
```

### 2. Test Database Connectivity

```bash
# PostgreSQL
createdb test_db
psql test_db -c "CREATE TABLE test (id SERIAL PRIMARY KEY, name VARCHAR(50));"
psql test_db -c "INSERT INTO test (name) VALUES ('test');"
psql test_db -c "SELECT * FROM test;"
dropdb test_db

# Redis
redis-cli ping
redis-cli set test_key "test_value"
redis-cli get test_key
redis-cli del test_key
```

### 3. Test Development Workflow

```bash
# Test Git workflow
mkdir test_git && cd test_git
git init
echo "# Test" > README.md
git add README.md
git commit -m "Initial commit"
git log --oneline
cd .. && rm -rf test_git

# Test Node.js
node -e "console.log('Node.js version:', process.version)"
npm --version
```

### 4. Test Docker Environment

```bash
# Test Docker
docker run --rm hello-world

# Test docker-compose
cat > docker-compose.yml << EOF
version: '3'
services:
  test:
    image: alpine:latest
    command: echo "Docker Compose OK"
EOF

docker-compose up test
rm docker-compose.yml
```

## Controlli di Performance

### Disk Space

```bash
# Verifica spazio disco
df -h

# Verifica spazio in home directory
du -sh ~

# Verifica cache Docker (se applicabile)
docker system df
```

### Memory Usage

```bash
# Verifica memoria
free -h

# Top processi per memoria
ps aux --sort=-%mem | head -10
```

### Ruby Performance Test

```bash
# Benchmark Ruby
ruby -e "
require 'benchmark'
n = 1000000
Benchmark.bm do |x|
  x.report('Array creation:') { n.times { [] } }
  x.report('Hash creation:') { n.times { {} } }
end
"
```

## Test VS Code Extensions

```bash
# Lista estensioni installate
code --list-extensions

# Test estensioni essenziali
REQUIRED_EXTENSIONS=(
    "shopify.ruby-lsp"
    "bung87.rails"
    "eamodio.gitlens"
    "ms-azuretools.vscode-docker"
)

echo "Verifica estensioni VS Code:"
for ext in "${REQUIRED_EXTENSIONS[@]}"; do
    if code --list-extensions | grep -q "$ext"; then
        echo "✓ $ext installato"
    else
        echo "✗ $ext mancante"
    fi
done
```

## Health Check Project Rails

Crea un progetto di test completo:

```bash
#!/bin/bash
# health_check_rails.sh

echo "🚀 Test ambiente Rails completo..."

# Crea progetto test
rails new health_check_app --api --database=postgresql --skip-git

cd health_check_app

# Configura database
echo "
development:
  adapter: postgresql
  encoding: unicode
  database: health_check_development
  pool: 5
  username: $USER
  password: password
  host: localhost
" > config/database.yml

# Setup database
rails db:create
rails db:migrate

# Genera model e controller test
rails generate model Article title:string content:text
rails generate controller Articles index show create

# Migrate
rails db:migrate

# Test routes
echo "
Rails.application.routes.draw do
  resources :articles, only: [:index, :show, :create]
end
" > config/routes.rb

# Test controller
echo "
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
    render json: @articles
  end

  def show
    @article = Article.find(params[:id])
    render json: @article
  end

  def create
    @article = Article.new(article_params)
    if @article.save
      render json: @article, status: :created
    else
      render json: @article.errors, status: :unprocessable_entity
    end
  end

  private

  def article_params
    params.require(:article).permit(:title, :content)
  end
end
" > app/controllers/articles_controller.rb

# Test seed
echo "
Article.create!(title: 'Test Article', content: 'This is a test article.')
" > db/seeds.rb

rails db:seed

# Test server in background
rails server -d

# Wait for server
sleep 5

# Test API endpoints
echo "Testing API endpoints..."
curl -s http://localhost:3000/articles | jq '.'

# Test POST
curl -s -X POST http://localhost:3000/articles \
  -H "Content-Type: application/json" \
  -d '{"article":{"title":"New Article","content":"New content"}}' | jq '.'

# Stop server
pkill -f "rails server"

# Cleanup
cd ..
rm -rf health_check_app

echo "✅ Test Rails completo terminato!"
```

## Automatizzazione Controlli

Aggiungi al tuo `.bashrc` o `.zshrc`:

```bash
# Alias per verifica ambiente
alias check-env='bash ~/check_env.sh'
alias check-rails='bash ~/health_check_rails.sh'

# Quick health check
alias dev-status='echo "Ruby: $(ruby --version | cut -d" " -f2)" && echo "Rails: $(rails --version | cut -d" " -f2)" && echo "Node: $(node --version)" && echo "Docker: $(docker --version | cut -d" " -f3 | tr -d ",")"'
```

Questo permette di eseguire rapidamente:

```bash
check-env        # Verifica completa ambiente
check-rails      # Test progetto Rails
dev-status       # Status rapido versioni
