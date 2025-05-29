# Configurazione Prettier

Configurazione completa Prettier per formattazione automatica del codice JavaScript nei progetti PANDEV.

## 🎨 Setup Base

### Installazione

```bash
# Core Prettier
npm install --save-dev prettier

# ESLint integration
npm install --save-dev eslint-config-prettier
npm install --save-dev eslint-plugin-prettier
```

### File .prettierrc Standard PANDEV

```json
{
  "semi": false,
  "singleQuote": true,
  "quoteProps": "as-needed",
  "trailingComma": "none",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "avoid",
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "endOfLine": "lf",
  "embeddedLanguageFormatting": "auto"
}
```

### File .prettierignore

```gitignore
# Dependencies
node_modules/
package-lock.json
yarn.lock

# Build outputs
dist/
build/
coverage/
.next/
out/
.nuxt/
.output/

# Logs
*.log

# OS files
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/

# Temporary
tmp/
temp/

# Generated files
*.min.js
*.min.css

# Framework specifici
# Angular
*.tsbuildinfo

# Vue
.vue/

# Documentation
docs/.vuepress/dist/
```

## 🎨 Configurazioni Framework-Specific

### React/JSX Projects

```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "none",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "avoid",
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "endOfLine": "lf",
  "jsxSingleQuote": true,
  "jsxBracketSameLine": false
}
```

### Vue Projects

```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "none",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "avoid",
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "endOfLine": "lf",
  "vueIndentScriptAndStyle": false,
  "overrides": [
    {
      "files": "*.vue",
      "options": {
        "parser": "vue"
      }
    }
  ]
}
```

### TypeScript Projects

```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "none",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "avoid",
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "endOfLine": "lf",
  "overrides": [
    {
      "files": "*.ts",
      "options": {
        "parser": "typescript"
      }
    },
    {
      "files": "*.tsx",
      "options": {
        "parser": "typescript",
        "jsxSingleQuote": true
      }
    }
  ]
}
```

## 📁 Configurazioni Multi-Linguaggio

### Full Stack Project

```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "none",
  "bracketSpacing": true,
  "bracketSameLine": false,
  "arrowParens": "avoid",
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "endOfLine": "lf",
  "overrides": [
    {
      "files": "*.json",
      "options": {
        "printWidth": 120
      }
    },
    {
      "files": "*.md",
      "options": {
        "printWidth": 80,
        "proseWrap": "always"
      }
    },
    {
      "files": "*.yml",
      "options": {
        "tabWidth": 2,
        "singleQuote": false
      }
    },
    {
      "files": "*.css",
      "options": {
        "singleQuote": false
      }
    },
    {
      "files": "*.scss",
      "options": {
        "singleQuote": false
      }
    },
    {
      "files": "*.html",
      "options": {
        "printWidth": 120
      }
    }
  ]
}
```

## 🛠️ Integration con Tools

### Package.json Scripts

```json
{
  "scripts": {
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "format:js": "prettier --write 'src/**/*.{js,jsx,ts,tsx}'",
    "format:css": "prettier --write 'src/**/*.{css,scss,less}'",
    "format:md": "prettier --write '**/*.md'"
  }
}
```

### ESLint Integration

```javascript
// .eslintrc.js con Prettier
module.exports = {
  extends: [
    'standard',
    'prettier' // Deve essere l'ultimo
  ],
  plugins: ['prettier'],
  rules: {
    'prettier/prettier': 'error'
  }
}
```

### Verifica Conflitti ESLint/Prettier

```bash
# Installa tool di verifica
npm install --save-dev eslint-config-prettier

# Controlla conflitti
npx eslint-config-prettier src/index.js
```

## 🔧 VS Code Integration

### Workspace Settings

```json
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnPaste": true,
  "editor.formatOnType": false,
  "prettier.requireConfig": true,
  "prettier.useEditorConfig": false,
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[markdown]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.wordWrap": "on"
  }
}
```

### User Settings (Global)

```json
{
  "prettier.semi": false,
  "prettier.singleQuote": true,
  "prettier.trailingComma": "none",
  "prettier.printWidth": 100,
  "prettier.tabWidth": 2,
  "prettier.arrowParens": "avoid"
}
```

## 🪝 Pre-commit Hooks

### Husky + lint-staged Setup

```bash
npm install --save-dev husky lint-staged
npx husky install
npx husky add .husky/pre-commit "npx lint-staged"
```

### lint-staged Configuration

```json
// package.json
{
  "lint-staged": {
    "*.{js,jsx,ts,tsx,vue}": [
      "eslint --fix",
      "prettier --write"
    ],
    "*.{css,scss,less}": [
      "prettier --write"
    ],
    "*.{json,md,yml,yaml}": [
      "prettier --write"
    ]
  }
}
```

### Commit Hook Solo Prettier

```json
// package.json per solo formatting
{
  "lint-staged": {
    "*.{js,jsx,ts,tsx,vue,css,scss,json,md}": [
      "prettier --write"
    ]
  }
}
```

## 🔄 CI/CD Integration

### GitHub Actions

```yaml
# .github/workflows/format-check.yml
name: Format Check

on: [push, pull_request]

jobs:
  format-check:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Check Prettier formatting
        run: npm run format:check
      
      - name: Comment PR if formatting issues
        if: failure()
        uses: actions/github-script@v6
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: '❌ Code formatting issues found. Run `npm run format` to fix.'
            })
```

### Auto-format Action

```yaml
# .github/workflows/auto-format.yml
name: Auto Format

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  auto-format:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run Prettier
        run: npm run format
      
      - name: Commit changes
        uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit_message: 'style: auto-format code with prettier'
```

## ⚙️ Configurazioni Avanzate

### EditorConfig Integration

```ini
# .editorconfig
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
indent_style = space
indent_size = 2

[*.md]
trim_trailing_whitespace = false

[*.{yml,yaml}]
indent_size = 2
```

### Prettier con EditorConfig

```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "none",
  "printWidth": 100,
  "useEditorConfig": true
}
```

### Ignore Patterns Avanzati

```javascript
// prettier.config.js per configurazioni complesse
module.exports = {
  semi: false,
  singleQuote: true,
  trailingComma: 'none',
  printWidth: 100,
  tabWidth: 2,
  overrides: [
    {
      files: '*.json',
      options: {
        printWidth: 120,
        tabWidth: 2
      }
    },
    {
      files: ['*.js', '*.jsx'],
      options: {
        arrowParens: 'avoid'
      }
    },
    {
      files: '*.ts',
      options: {
        arrowParens: 'always'
      }
    }
  ]
}
```

## 🔧 IDE Integration

### WebStorm/IntelliJ

```json
// File > Settings > Languages & Frameworks > Prettier
{
  "prettierPath": "./node_modules/prettier",
  "configPath": "./.prettierrc",
  "runOnSave": true,
  "runOnReformat": true
}
```

### Vim/Neovim

```lua
-- Prettier setup per Neovim
require('prettier').setup({
  bin = 'prettier',
  filetypes = {
    "css",
    "javascript",
    "typescript",
    "json",
    "vue",
    "markdown"
  }
})
```

## 🐛 Troubleshooting

### Problemi Comuni

**Prettier non formatta:**
```bash
# Verifica configurazione
npx prettier --check src/index.js

# Debug
npx prettier --debug-check src/index.js
```

**Conflitti con ESLint:**
```bash
# Usa eslint-config-prettier
npm install --save-dev eslint-config-prettier

# Verifica conflitti
npx eslint-config-prettier src/index.js
```

**VS Code non formatta:**
```json
// Verifica settings.json
{
  "prettier.requireConfig": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```

**File non inclusi:**
```bash
# Controlla .prettierignore
npx prettier --write --loglevel debug src/
```

## 📊 Best Practices

### Team Workflow

1. **Configurazione condivisa** tramite `.prettierrc`
2. **Pre-commit hooks** per enforcement automatico
3. **CI checks** per verificare formatting
4. **Editor integration** per tutti i team members
5. **Documentation** delle regole di formatting

### Performance Tips

```javascript
// prettier.config.js ottimizzato
module.exports = {
  // ... configurazione base
  
  // Ignora file grandi
  ignorePath: '.prettierignore',
  
  // Cache per performance
  cache: true,
  cacheLocation: '.prettier-cache'
}
```

Questa configurazione Prettier garantisce formattazione consistente e automatica per tutti i progetti JavaScript PANDEV.
