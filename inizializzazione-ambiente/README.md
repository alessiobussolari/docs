# 🚀 Inizializzazione Ambiente di Sviluppo

Questa sezione contiene guide complete per configurare un ambiente di sviluppo ottimizzato per i progetti PANDEV.

## Sistemi Operativi Supportati

- [**macOS**](setup-macos.md) - Setup automatico completo (raccomandato)
- [**Windows**](setup-windows.md) - Configurazione per Windows 10/11
- [**Linux**](setup-linux.md) - Setup per distribuzioni Ubuntu/Debian

## Guide Specifiche

- [**Verifica Ambiente**](verifica-ambiente.md) - Controlli post-installazione
- [**Troubleshooting**](troubleshooting.md) - Risoluzione problemi comuni
- [**Aggiornamenti**](aggiornamenti.md) - Mantenimento dell'ambiente

## Overview degli Strumenti

Il nostro stack di sviluppo include:

### Core Development
- **Homebrew** - Package manager per macOS
- **Git** - Controllo versione con configurazioni avanzate
- **asdf** - Version manager universale per linguaggi
- **VS Code** - Editor principale con estensioni essenziali

### Ruby/Rails Specifico
- **Ruby** - Ultima versione stabile
- **Bundler** - Gestione dipendenze Ruby
- **PostgreSQL** - Database principale
- **Redis** - Cache e background jobs

### CLI Utilities
- **iTerm2** - Terminal avanzato per macOS
- **HTTPie** - Client HTTP user-friendly
- **jq** - Processor JSON per CLI
- **git-delta** - Diff colorati migliorati
- **fzf** - Fuzzy finder veloce
- **bat** - Sostituto di cat con syntax highlighting
- **tree** - Visualizzazione directory ad albero

### Container & Deploy
- **Docker Desktop** - Containerizzazione
- **docker-compose** - Orchestrazione container
- **Overmind** - Process manager (alternativa a Foreman)

## Quick Start

Per un setup rapido su macOS:

```bash
# Clona il repository setup
git clone https://bitbucket.org/pandev-srl/setup-dev-mac.git
cd setup-dev-mac

# Esegui lo script di setup
./setup.sh
```

Segui poi la [guida dettagliata per macOS](setup-macos.md) per configurazioni avanzate.
