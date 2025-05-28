# Configurazione RuboCop

Configurazione standard RuboCop per progetti PANDEV con regole ottimizzate per Rails API.

## File .rubocop.yml Standard

Crea questo file nella root del progetto:

```yaml
# .rubocop.yml
require:
  - rubocop-rails
  - rubocop-rspec
  - rubocop-performance

AllCops:
  TargetRubyVersion: 3.1
  NewCops: enable
  Exclude:
    - 'vendor/**/*'
    - 'db/schema.rb'
    - 'db/migrate/*'
    - 'bin/*'
    - 'node_modules/**/*'
    - 'tmp/**/*'
    - 'log/**/*'
    - 'public/**/*'

# Layout
Layout/LineLength:
  Max: 120
  AllowedPatterns: ['(\A|\s)#']

Layout/MultilineMethodCallIndentation:
  EnforcedStyle: indented

Layout/FirstArrayElementIndentation:
  EnforcedStyle: consistent

Layout/FirstHashElementIndentation:
  EnforcedStyle: consistent

# Style
Style/Documentation:
  Enabled: false

Style/FrozenStringLiteralComment:
  Enabled: true
  EnforcedStyle: always

Style/StringLiterals:
  EnforcedStyle: single_quotes

Style/StringLiteralsInInterpolation:
  EnforcedStyle: single_quotes

Style/TrailingCommaInArrayLiteral:
  EnforcedStyleForMultiline: comma

Style/TrailingCommaInHashLiteral:
  EnforcedStyleForMultiline: comma

Style/TrailingCommaInArguments:
  EnforcedStyleForMultiline: comma

Style/ClassAndModuleChildren:
  EnforcedStyle: compact

Style/Lambda:
  EnforcedStyle: literal

# Metrics
Metrics/BlockLength:
  Exclude:
    - 'config/routes.rb'
    - 'spec/**/*'
    - 'config/environments/*'

Metrics/MethodLength:
  Max: 20
  CountAsOne: ['array', 'hash', 'heredoc']

Metrics/AbcSize:
  Max: 20

Metrics/ClassLength:
  Max: 150

Metrics/ModuleLength:
  Max: 150

Metrics/CyclomaticComplexity:
  Max: 8

Metrics/PerceivedComplexity:
  Max: 8

# Rails specific
Rails/FilePath:
  Enabled: false

Rails/UnknownEnv:
  Environments:
    - production
    - development
    - test
    - staging

Rails/SkipsModelValidations:
  Exclude:
    - 'spec/**/*'
    - 'db/migrate/*'

# RSpec specific
RSpec/ExampleLength:
  Max: 20

RSpec/MultipleExpectations:
  Max: 5

RSpec/NestedGroups:
  Max: 5

RSpec/DescribeClass:
  Exclude:
    - 'spec/requests/**/*'
    - 'spec/features/**/*'
    - 'spec/system/**/*'

# Performance
Performance/Casecmp:
  Enabled: true

Performance/StringReplacement:
  Enabled: true

# Naming
Naming/PredicateName:
  ForbiddenPrefixes:
    - is_

Naming/AccessorMethodName:
  Enabled: true

# Security
Security/Open:
  Enabled: true

Security/Eval:
  Enabled: true
```

## Installazione

### Gemfile

Aggiungi al tuo Gemfile:

```ruby
group :development, :test do
  gem 'rubocop', '~> 1.50', require: false
  gem 'rubocop-rails', '~> 2.19', require: false
  gem 'rubocop-rspec', '~> 2.20', require: false
  gem 'rubocop-performance', '~> 1.16', require: false
end
```

### Bundle Install

```bash
bundle install
```

## Comandi Utili

### Controllo Base

```bash
# Controlla tutto il progetto
bundle exec rubocop

# Controlla file specifici
bundle exec rubocop app/models/user.rb

# Controlla directory specifica
bundle exec rubocop app/controllers/
```

### Auto-correzione

```bash
# Auto-fix sicuro (solo whitespace, quotes, etc.)
bundle exec rubocop -a

# Auto-fix anche unsafe (più aggressivo)
bundle exec rubocop -A

# Auto-fix solo file specifici
bundle exec rubocop -a app/models/user.rb
```

### Configurazione Cop Specifici

```bash
# Disabilita cop specifico per un file
bundle exec rubocop --disable-uncorrectable

# Genera configurazione TODO (ignora violazioni esistenti)
bundle exec rubocop --auto-gen-config
```

## Integrazione Editor

### VS Code

Installa l'estensione **Ruby LSP** che include RuboCop automaticamente.

### Configurazione .vscode/settings.json

```json
{
  "ruby.lint": {
    "rubocop": {
      "useBundler": true
    }
  },
  "ruby.format": "rubocop",
  "[ruby]": {
    "editor.formatOnSave": true,
    "editor.formatOnType": true
  }
}
```

## Pre-commit Hook

Aggiungi al file `.git/hooks/pre-commit`:

```bash
#!/bin/sh
echo "Running RuboCop..."
bundle exec rubocop --fail-fast

if [ $? -ne 0 ]; then
  echo "RuboCop failed. Commit aborted."
  echo "Run 'bundle exec rubocop -a' to auto-fix issues."
  exit 1
fi
```

Rendi eseguibile:

```bash
chmod +x .git/hooks/pre-commit
```

## CI/CD Integration

### GitHub Actions

```yaml
# .github/workflows/rubocop.yml
name: RuboCop

on: [push, pull_request]

jobs:
  rubocop:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: 3.1
          bundler-cache: true
      - run: bundle exec rubocop
```

## Personalizzazioni PANDEV

### Override per Progetti Specifici

Crea `.rubocop_local.yml` per override specifici:

```yaml
# .rubocop_local.yml
inherit_from: .rubocop.yml

# Override specifici per questo progetto
Metrics/MethodLength:
  Max: 25

Style/FrozenStringLiteralComment:
  Enabled: false
```

Poi esegui:

```bash
bundle exec rubocop --config .rubocop_local.yml
```

### Disabilitare Cop per Codice Legacy

```ruby
# Disabilita per una riga
some_legacy_code # rubocop:disable Style/FrozenStringLiteralComment

# Disabilita per un blocco
# rubocop:disable Metrics/MethodLength
def legacy_method
  # codice lungo legacy
end
# rubocop:enable Metrics/MethodLength

# Disabilita per tutto il file
# rubocop:disable-file Metrics/ClassLength
```

## Best Practices

1. **Esegui RuboCop prima di ogni commit**
2. **Usa auto-fix con cautela** - controlla sempre le modifiche
3. **Non disabilitare cop senza motivo** - documenta sempre il perché
4. **Aggiorna regolarmente** le versioni delle gem RuboCop
5. **Condividi la configurazione** nel team per coerenza

## Troubleshooting

### Problemi Comuni

**RuboCop lento:**
```bash
# Usa cache per velocizzare
bundle exec rubocop --cache true
```

**Conflitti di configurazione:**
```bash
# Debug configurazione
bundle exec rubocop --show-cops Style/StringLiterals
```

**Versioni incompatibili:**
```bash
# Aggiorna tutte le gem RuboCop
bundle update rubocop rubocop-rails rubocop-rspec rubocop-performance
