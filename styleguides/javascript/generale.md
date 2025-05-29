# JavaScript Style Guide

Guida di stile JavaScript completa per progetti PANDEV, ottimizzata per React, Vue, Angular e Node.js.

## 🎯 Overview

JavaScript è il linguaggio principale per lo sviluppo frontend e sempre più utilizzato per backend nei progetti PANDEV. Questa guida copre:

- **Frontend**: React, Vue, Angular
- **Backend**: Node.js, Express
- **Tools**: ESLint, Prettier, Babel
- **Modern JS**: ES6+, async/await, modules

## 📋 Standard Adottati

### ESLint Standard
Utilizziamo **ESLint Standard** come base per il linting:

```json
{
  "extends": ["standard"],
  "env": {
    "browser": true,
    "node": true,
    "es2022": true
  }
}
```

### Prettier per Formatting
**Prettier** per la formattazione automatica del codice:

```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "none",
  "printWidth": 100
}
```

## 🛠️ Setup Rapido

### 1. Installazione Dependencies

```bash
# Core tools
npm install --save-dev eslint prettier

# ESLint Standard config
npm install --save-dev eslint-config-standard

# Prettier integration
npm install --save-dev eslint-config-prettier eslint-plugin-prettier

# Framework specifici (scegli quello che usi)
npm install --save-dev eslint-plugin-react      # React
npm install --save-dev eslint-plugin-vue        # Vue
npm install --save-dev @angular-eslint/eslint-plugin  # Angular
```

### 2. File di Configurazione

Crea `.eslintrc.js`:
```javascript
module.exports = {
  extends: [
    'standard',
    'prettier'
  ],
  plugins: ['prettier'],
  rules: {
    'prettier/prettier': 'error'
  }
}
```

Crea `.prettierrc`:
```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "none",
  "printWidth": 100,
  "tabWidth": 2
}
```

### 3. Scripts Package.json

```json
{
  "scripts": {
    "lint": "eslint src/**/*.js",
    "lint:fix": "eslint src/**/*.js --fix",
    "format": "prettier --write src/**/*.js"
  }
}
```

## 🎨 Framework Specifici

### React Projects
Configurazione specifica per React:

```javascript
// .eslintrc.js per React
module.exports = {
  extends: [
    'standard',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'prettier'
  ],
  plugins: ['react', 'react-hooks', 'prettier'],
  settings: {
    react: {
      version: 'detect'
    }
  }
}
```

### Vue Projects
Configurazione per Vue:

```javascript
// .eslintrc.js per Vue
module.exports = {
  extends: [
    'standard',
    'plugin:vue/vue3-essential',
    'prettier'
  ],
  plugins: ['vue', 'prettier']
}
```

### Angular Projects
Angular usa ESLint dalla v12+:

```json
{
  "extends": [
    "@angular-eslint/recommended",
    "standard",
    "prettier"
  ]
}
```

### Node.js Projects
Configurazione per backend Node.js:

```javascript
// .eslintrc.js per Node.js
module.exports = {
  extends: [
    'standard',
    'prettier'
  ],
  env: {
    node: true,
    es2022: true
  },
  rules: {
    'no-console': 'warn' // Permetti console.log in development
  }
}
```

## 📝 Principi Base

### 1. Modern JavaScript (ES6+)
- Usa `const` e `let` invece di `var`
- Preferisci arrow functions per callback
- Usa template literals per string interpolation
- Destructuring per oggetti e array
- Async/await invece di Promise chains

### 2. Naming Conventions
- **camelCase**: variabili, funzioni, metodi
- **PascalCase**: classi, costruttori, componenti
- **UPPER_SNAKE_CASE**: costanti
- **kebab-case**: file names, CSS classes

### 3. Code Organization
- Un componente/classe per file
- Imports ordinati (librerie, locali)
- Export espliciti
- Separazione logica con commenti

## 🔧 Tools Integration

### VS Code Setup
Installa estensioni consigliate:
- **ESLint** (Microsoft)
- **Prettier** (Prettier)
- **Auto Rename Tag** (Jun Han)
- **Bracket Pair Colorizer** (CoenraadS)

Configurazione workspace (`.vscode/settings.json`):
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "eslint.validate": ["javascript", "typescript", "vue"],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

### Pre-commit Hooks
Setup con Husky + lint-staged:

```bash
npm install --save-dev husky lint-staged
```

Package.json:
```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{js,vue,ts}": [
      "eslint --fix",
      "prettier --write"
    ]
  }
}
```

## 📊 Qualità del Codice

### Metrics da Monitorare
- **ESLint errors**: 0 errori in production
- **Code coverage**: > 80%
- **Cyclomatic complexity**: < 10 per funzione
- **Function length**: < 20 linee
- **File size**: < 300 linee

### Code Review Checklist
- [ ] ESLint warnings risolti
- [ ] Prettier formatting applicato
- [ ] Naming conventions rispettate
- [ ] No console.log in production
- [ ] Error handling implementato
- [ ] Comments per logica complessa
- [ ] Tests aggiunti/aggiornati

## 🚀 Performance

### Best Practices
- **Evita re-renders** inutili (React.memo, useMemo)
- **Lazy loading** per componenti pesanti
- **Tree shaking** per bundle size
- **Code splitting** per route
- **Debounce** per input frequenti

### Bundle Analysis
```bash
# React
npm install --save-dev webpack-bundle-analyzer
npm run build -- --analyze

# Vue
vue-cli-service build --report

# Angular
ng build --stats-json
npx webpack-bundle-analyzer dist/stats.json
```

## 📚 Risorse

### Documentazione Ufficiale
- [ESLint Rules](https://eslint.org/docs/rules/)
- [Prettier Options](https://prettier.io/docs/en/options.html)
- [JavaScript Standard Style](https://standardjs.com/)

### Framework Guides
- [React ESLint Plugin](https://github.com/yannickcr/eslint-plugin-react)
- [Vue ESLint Plugin](https://eslint.vuejs.org/)
- [Angular ESLint](https://github.com/angular-eslint/angular-eslint)

Per configurazioni dettagliate e regole specifiche, consulta le sezioni dedicate:
- [Configurazione ESLint](configurazione-eslint.md)
- [Configurazione Prettier](configurazione-prettier.md)
- [Configurazione VS Code](configurazione-vscode.md)
- [Performance Guidelines](performance.md)
- [Regole Specifiche](regole/README.md)
