# Configurazione ESLint

Configurazione completa ESLint per progetti JavaScript PANDEV con Standard Style e supporto multi-framework.

## 🛠️ Setup Base

### Installazione Dependencies

```bash
# Core ESLint
npm install --save-dev eslint

# Standard config
npm install --save-dev eslint-config-standard
npm install --save-dev eslint-plugin-import
npm install --save-dev eslint-plugin-node
npm install --save-dev eslint-plugin-promise

# Prettier integration
npm install --save-dev eslint-config-prettier
npm install --save-dev eslint-plugin-prettier
```

### File .eslintrc.js Standard

```javascript
module.exports = {
  root: true,
  extends: [
    'standard',
    'prettier'
  ],
  plugins: ['prettier'],
  env: {
    browser: true,
    node: true,
    es2022: true
  },
  parserOptions: {
    ecmaVersion: 2022,
    sourceType: 'module'
  },
  rules: {
    // Prettier integration
    'prettier/prettier': 'error',
    
    // Custom PANDEV rules
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'warn',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'warn',
    
    // Code quality
    'prefer-const': 'error',
    'no-var': 'error',
    'object-shorthand': 'error',
    'prefer-arrow-callback': 'error',
    'prefer-template': 'error',
    
    // Import organization
    'import/order': [
      'error',
      {
        groups: [
          'builtin',
          'external',
          'internal',
          'parent',
          'sibling',
          'index'
        ],
        'newlines-between': 'always'
      }
    ]
  }
}
```

## 🎨 Configurazioni Framework-Specific

### React Projects

```bash
# React dependencies
npm install --save-dev eslint-plugin-react
npm install --save-dev eslint-plugin-react-hooks
npm install --save-dev eslint-plugin-jsx-a11y
```

```javascript
// .eslintrc.js per React
module.exports = {
  root: true,
  extends: [
    'standard',
    'plugin:react/recommended',
    'plugin:react-hooks/recommended',
    'plugin:jsx-a11y/recommended',
    'prettier'
  ],
  plugins: ['react', 'react-hooks', 'jsx-a11y', 'prettier'],
  env: {
    browser: true,
    es2022: true
  },
  parserOptions: {
    ecmaVersion: 2022,
    sourceType: 'module',
    ecmaFeatures: {
      jsx: true
    }
  },
  settings: {
    react: {
      version: 'detect'
    }
  },
  rules: {
    'prettier/prettier': 'error',
    
    // React specific
    'react/prop-types': 'off', // Se usi TypeScript
    'react/react-in-jsx-scope': 'off', // React 17+
    'react/jsx-uses-react': 'off', // React 17+
    'react/jsx-filename-extension': ['error', { extensions: ['.jsx', '.tsx'] }],
    
    // Hooks rules
    'react-hooks/rules-of-hooks': 'error',
    'react-hooks/exhaustive-deps': 'warn',
    
    // JSX accessibility
    'jsx-a11y/anchor-is-valid': 'off', // Next.js Link component
    
    // Console in development
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'warn'
  }
}
```

### Vue Projects

```bash
# Vue dependencies
npm install --save-dev eslint-plugin-vue
```

```javascript
// .eslintrc.js per Vue
module.exports = {
  root: true,
  extends: [
    'standard',
    'plugin:vue/vue3-essential', // o vue3-strongly-recommended, vue3-recommended
    'prettier'
  ],
  plugins: ['vue', 'prettier'],
  env: {
    browser: true,
    node: true,
    es2022: true
  },
  parserOptions: {
    ecmaVersion: 2022,
    sourceType: 'module'
  },
  rules: {
    'prettier/prettier': 'error',
    
    // Vue specific
    'vue/html-self-closing': 'off',
    'vue/max-attributes-per-line': 'off',
    'vue/singleline-html-element-content-newline': 'off',
    'vue/component-name-in-template-casing': ['error', 'PascalCase'],
    
    // Console
    'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'warn'
  }
}
```

### Angular Projects

```bash
# Angular ESLint
ng add @angular-eslint/schematics

# Dependencies aggiuntive
npm install --save-dev eslint-config-prettier
npm install --save-dev eslint-plugin-prettier
```

```json
// .eslintrc.json per Angular
{
  "root": true,
  "ignorePatterns": ["projects/**/*"],
  "overrides": [
    {
      "files": ["*.ts"],
      "extends": [
        "@angular-eslint/recommended",
        "@angular-eslint/template/process-inline-templates",
        "prettier"
      ],
      "plugins": ["prettier"],
      "rules": {
        "prettier/prettier": "error",
        "@angular-eslint/directive-selector": [
          "error",
          {
            "type": "attribute",
            "prefix": "app",
            "style": "camelCase"
          }
        ],
        "@angular-eslint/component-selector": [
          "error",
          {
            "type": "element",
            "prefix": "app",
            "style": "kebab-case"
          }
        ]
      }
    },
    {
      "files": ["*.html"],
      "extends": ["@angular-eslint/template/recommended"],
      "rules": {}
    }
  ]
}
```

### Node.js/Express Projects

```javascript
// .eslintrc.js per Node.js
module.exports = {
  root: true,
  extends: [
    'standard',
    'prettier'
  ],
  plugins: ['prettier'],
  env: {
    node: true,
    es2022: true
  },
  parserOptions: {
    ecmaVersion: 2022,
    sourceType: 'module'
  },
  rules: {
    'prettier/prettier': 'error',
    
    // Node.js specific
    'no-console': 'off', // Console permesso in Node.js
    'node/no-unpublished-require': 'off',
    'node/no-missing-require': 'error',
    
    // Security
    'security/detect-object-injection': 'warn'
  }
}
```

## 📁 File .eslintignore

```gitignore
# Dependencies
node_modules/
npm-debug.log*

# Build outputs
dist/
build/
coverage/

# Framework specifici
# React
.next/
out/

# Vue
.nuxt/
.output/

# Angular
*.tsbuildinfo

# Logs
*.log

# OS
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/

# Temporary
tmp/
temp/
```

## 🚀 Scripts Package.json

```json
{
  "scripts": {
    "lint": "eslint src --ext .js,.jsx,.vue,.ts,.tsx",
    "lint:fix": "eslint src --ext .js,.jsx,.vue,.ts,.tsx --fix",
    "lint:ci": "eslint src --ext .js,.jsx,.vue,.ts,.tsx --format junit --output-file reports/eslint.xml"
  }
}
```

## 🔧 VS Code Integration

### Workspace Settings

```json
// .vscode/settings.json
{
  "eslint.validate": [
    "javascript",
    "typescript",
    "vue",
    "html"
  ],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.workingDirectories": [
    "."
  ]
}
```

### Extensions Consigliate

```json
// .vscode/extensions.json
{
  "recommendations": [
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "ms-vscode.vscode-typescript-next"
  ]
}
```

## 🪝 Pre-commit Hooks

### Setup Husky + lint-staged

```bash
# Installazione
npm install --save-dev husky lint-staged

# Setup husky
npx husky install
npx husky add .husky/pre-commit "npx lint-staged"
```

### Configurazione lint-staged

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
    "*.{json,md}": [
      "prettier --write"
    ]
  }
}
```

## 🔄 CI/CD Integration

### GitHub Actions

```yaml
# .github/workflows/lint.yml
name: Lint

on: [push, pull_request]

jobs:
  lint:
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
      
      - name: Run ESLint
        run: npm run lint
      
      - name: Upload ESLint results
        uses: actions/upload-artifact@v3
        if: failure()
        with:
          name: eslint-results
          path: reports/
```

## 🎛️ Configurazioni Avanzate

### Performance per Large Codebases

```javascript
// .eslintrc.js ottimizzato
module.exports = {
  // ... altre configurazioni
  
  // Cache per performance
  cache: true,
  cacheLocation: '.eslintcache',
  
  // Parser options ottimizzati
  parserOptions: {
    ecmaVersion: 2022,
    sourceType: 'module',
    allowImportExportEverywhere: false,
    ecmaFeatures: {
      globalReturn: false
    }
  }
}
```

### Override per File Specifici

```javascript
module.exports = {
  // ... base config
  
  overrides: [
    {
      files: ['**/*.test.js', '**/*.spec.js'],
      env: {
        jest: true
      },
      rules: {
        'no-console': 'off'
      }
    },
    {
      files: ['**/*.config.js'],
      env: {
        node: true
      },
      rules: {
        'no-console': 'off'
      }
    }
  ]
}
```

## 🐛 Troubleshooting

### Problemi Comuni

**ESLint non trova configurazione:**
```bash
# Verifica percorso
npx eslint --print-config src/index.js

# Debug
npx eslint --debug src/index.js
```

**Conflitti Prettier/ESLint:**
```bash
# Verifica conflitti
npx eslint-config-prettier src/index.js
```

**Performance lente:**
```bash
# Usa cache
npx eslint --cache src/

# Parallel processing
npm install --save-dev eslint-parallel
```

**Plugin non trovati:**
```bash
# Installa tutte le peer dependencies
npx install-peerdeps --dev eslint-config-standard
```

Questa configurazione ESLint garantisce codice consistente e di qualità per tutti i progetti JavaScript PANDEV.
