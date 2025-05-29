# Configurazione VS Code per JavaScript

Configurazione completa VS Code per sviluppo JavaScript ottimizzata per progetti PANDEV con React, Vue, Angular e Node.js.

## 🛠️ Estensioni Essential

### Core Extensions

```json
// .vscode/extensions.json
{
  "recommendations": [
    // JavaScript/TypeScript
    "ms-vscode.vscode-typescript-next",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    
    // Code Quality
    "streetsidesoftware.code-spell-checker",
    "usernamehw.errorlens",
    "ms-vscode.vscode-json",
    
    // Git
    "eamodio.gitlens",
    "mhutchie.git-graph",
    
    // Utils
    "christian-kohler.path-intellisense",
    "formulahendry.auto-rename-tag",
    "bradlc.vscode-tailwindcss"
  ]
}
```

### Framework-Specific Extensions

```json
// React Projects
{
  "recommendations": [
    "ms-vscode.vscode-typescript-next",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss",
    "dsznajder.es7-react-js-snippets",
    "ms-vscode.vscode-react-native"
  ]
}

// Vue Projects
{
  "recommendations": [
    "Vue.volar",
    "Vue.vscode-typescript-vue-plugin",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "bradlc.vscode-tailwindcss"
  ]
}

// Angular Projects
{
  "recommendations": [
    "Angular.ng-template",
    "ms-vscode.vscode-typescript-next",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "johnpapa.angular2"
  ]
}

// Node.js Projects
{
  "recommendations": [
    "ms-vscode.vscode-typescript-next",
    "dbaeumer.vscode-eslint",
    "esbenp.prettier-vscode",
    "ms-vscode.vscode-node-debug2",
    "formulahendry.code-runner"
  ]
}
```

## ⚙️ Workspace Settings

### Configurazione Base

```json
// .vscode/settings.json
{
  // Editor
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "editor.formatOnType": false,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.detectIndentation": false,
  
  // Files
  "files.autoSave": "onFocusChange",
  "files.trimTrailingWhitespace": true,
  "files.insertFinalNewline": true,
  "files.trimFinalNewlines": true,
  
  // Search
  "search.exclude": {
    "**/node_modules": true,
    "**/dist": true,
    "**/build": true,
    "**/.next": true,
    "**/coverage": true
  },
  
  // File Watcher
  "files.watcherExclude": {
    "**/node_modules/**": true,
    "**/dist/**": true,
    "**/build/**": true
  }
}
```

### Language-Specific Settings

```json
{
  // JavaScript
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.suggest.insertMode": "replace",
    "editor.inlayHints.enabled": "on"
  },
  
  // TypeScript
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.suggest.insertMode": "replace",
    "editor.inlayHints.enabled": "on"
  },
  
  // JSX/TSX
  "[javascriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "emmet.includeLanguages": {
      "javascriptreact": "html"
    }
  },
  
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "emmet.includeLanguages": {
      "typescriptreact": "html"
    }
  },
  
  // Vue
  "[vue]": {
    "editor.defaultFormatter": "Vue.volar"
  },
  
  // JSON
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.quickSuggestions": {
      "strings": true
    }
  },
  
  // CSS/SCSS
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  
  "[scss]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

### ESLint Configuration

```json
{
  // ESLint
  "eslint.enable": true,
  "eslint.validate": [
    "javascript",
    "typescript",
    "javascriptreact",
    "typescriptreact",
    "vue"
  ],
  "eslint.workingDirectories": ["."],
  "eslint.codeActionsOnSave.mode": "all",
  "eslint.format.enable": true,
  "eslint.lintTask.enable": true,
  
  // Prettier
  "prettier.enable": true,
  "prettier.requireConfig": true,
  "prettier.useEditorConfig": false,
  
  // TypeScript
  "typescript.updateImportsOnFileMove.enabled": "always",
  "typescript.suggest.autoImports": true,
  "typescript.preferences.includePackageJsonAutoImports": "on",
  "typescript.inlayHints.parameterNames.enabled": "all",
  "typescript.inlayHints.functionLikeReturnTypes.enabled": true,
  "typescript.inlayHints.propertyDeclarationTypes.enabled": true
}
```

## 🎨 Framework-Specific Configurations

### React Development

```json
// .vscode/settings.json per React
{
  "emmet.includeLanguages": {
    "javascriptreact": "html",
    "typescriptreact": "html"
  },
  "emmet.triggerExpansionOnTab": true,
  
  // React specifics
  "typescript.preferences.includePackageJsonAutoImports": "on",
  "typescript.suggest.autoImports": true,
  
  // Snippets
  "editor.snippetSuggestions": "top",
  
  // IntelliSense
  "typescript.validate.enable": true,
  "javascript.validate.enable": false
}
```

### Vue Development

```json
// .vscode/settings.json per Vue
{
  // Vue Volar
  "vue.autoInsert.dotValue": true,
  "vue.autoInsert.parentheses": true,
  "vue.codeActions.enabled": true,
  "vue.complete.casing.props": "camel",
  "vue.complete.casing.tags": "pascal",
  
  // TypeScript in Vue
  "typescript.preferences.includePackageJsonAutoImports": "on",
  "typescript.suggest.autoImports": true,
  
  // Vetur (legacy - se non usi Volar)
  "vetur.format.defaultFormatter.html": "prettier",
  "vetur.format.defaultFormatter.js": "prettier-eslint",
  "vetur.validation.template": false
}
```

### Angular Development

```json
// .vscode/settings.json per Angular
{
  // Angular Language Service
  "angular.enable-strict-mode-prompt": false,
  "angular.experimental-ivy": true,
  
  // TypeScript
  "typescript.preferences.includePackageJsonAutoImports": "on",
  "typescript.suggest.autoImports": true,
  "typescript.updateImportsOnFileMove.enabled": "always",
  
  // HTML
  "html.suggest.html5": true,
  
  // Files associations
  "files.associations": {
    "*.component.html": "html",
    "*.component.css": "css"
  }
}
```

### Node.js Development

```json
// .vscode/settings.json per Node.js
{
  // Node.js specifics
  "node.suggest.autoImports": true,
  "npm.enableScriptExplorer": true,
  
  // Debug
  "debug.node.autoAttach": "smart",
  "debug.javascript.autoAttachFilter": "smart",
  
  // Terminal
  "terminal.integrated.env.osx": {
    "NODE_ENV": "development"
  },
  "terminal.integrated.env.linux": {
    "NODE_ENV": "development"
  },
  "terminal.integrated.env.windows": {
    "NODE_ENV": "development"
  }
}
```

## 🔧 Debugging Configuration

### Launch.json for Different Frameworks

```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch React Dev Server",
      "type": "node",
      "request": "launch",
      "cwd": "${workspaceFolder}",
      "runtimeExecutable": "npm",
      "runtimeArgs": ["start"]
    },
    {
      "name": "Launch Vue Dev Server",
      "type": "node",
      "request": "launch",
      "cwd": "${workspaceFolder}",
      "runtimeExecutable": "npm",
      "runtimeArgs": ["run", "serve"]
    },
    {
      "name": "Launch Angular Dev Server",
      "type": "node",
      "request": "launch",
      "cwd": "${workspaceFolder}",
      "runtimeExecutable": "npm",
      "runtimeArgs": ["start"]
    },
    {
      "name": "Launch Node.js App",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/src/index.js",
      "console": "integratedTerminal",
      "env": {
        "NODE_ENV": "development"
      }
    },
    {
      "name": "Attach to Chrome",
      "type": "chrome",
      "request": "attach",
      "port": 9222,
      "webRoot": "${workspaceFolder}/src",
      "sourceMaps": true
    }
  ]
}
```

## 🧪 Testing Configuration

### Jest Integration

```json
// .vscode/settings.json
{
  // Jest
  "jest.enable": true,
  "jest.autoRun": "watch",
  "jest.showCoverageOnLoad": true,
  "jest.coverageFormatter": "DefaultFormatter",
  
  // Test Explorer
  "testExplorer.useNativeTesting": true,
  
  // Files associations
  "files.associations": {
    "*.test.js": "javascript",
    "*.spec.js": "javascript",
    "*.test.ts": "typescript",
    "*.spec.ts": "typescript"
  }
}
```

## 🎨 Themes e UI

### Configurazioni UI Consigliate

```json
{
  // Theme
  "workbench.colorTheme": "One Dark Pro Darker",
  "workbench.iconTheme": "material-icon-theme",
  
  // Editor
  "editor.fontFamily": "'Fira Code', 'JetBrains Mono', 'Source Code Pro', monospace",
  "editor.fontLigatures": true,
  "editor.fontSize": 14,
  "editor.lineHeight": 1.5,
  "editor.minimap.enabled": false,
  "editor.renderWhitespace": "boundary",
  "editor.rulers": [80, 100],
  
  // Bracket colorization
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs": true,
  
  // Indentation guides
  "editor.renderIndentGuides": true,
  "editor.highlightActiveIndentGuide": true
}
```

## 🚀 Productivity Settings

### Code Navigation

```json
{
  // File navigation
  "workbench.editor.enablePreview": false,
  "workbench.editor.enablePreviewFromQuickOpen": false,
  "explorer.confirmDelete": false,
  "explorer.confirmDragAndDrop": false,
  
  // Quick suggestions
  "editor.quickSuggestions": {
    "other": true,
    "comments": false,
    "strings": true
  },
  "editor.suggestSelection": "first",
  "editor.acceptSuggestionOnCommitCharacter": false,
  
  // IntelliSense
  "editor.wordBasedSuggestions": true,
  "editor.parameterHints.enabled": true
}
```

### Auto-save e File Management

```json
{
  // Auto-save
  "files.autoSave": "onFocusChange",
  "files.autoSaveDelay": 1000,
  
  // File exclude patterns
  "files.exclude": {
    "**/node_modules": true,
    "**/.git": true,
    "**/.DS_Store": true,
    "**/dist": true,
    "**/build": true,
    "**/*.log": true
  },
  
  // Search settings
  "search.smartCase": true,
  "search.globalFindClipboard": true
}
```

## 🔨 Tasks Configuration

### Tasks.json for Common Operations

```json
// .vscode/tasks.json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "npm: install",
      "type": "shell",
      "command": "npm install",
      "group": "build",
      "presentation": {
        "reveal": "silent"
      },
      "problemMatcher": []
    },
    {
      "label": "npm: start",
      "type": "shell",
      "command": "npm start",
      "group": "build",
      "isBackground": true,
      "presentation": {
        "reveal": "always",
        "panel": "new"
      }
    },
    {
      "label": "npm: build",
      "type": "shell",
      "command": "npm run build",
      "group": "build",
      "presentation": {
        "reveal": "silent"
      },
      "problemMatcher": []
    },
    {
      "label": "npm: test",
      "type": "shell",
      "command": "npm test",
      "group": "test",
      "presentation": {
        "reveal": "always"
      }
    },
    {
      "label": "lint:fix",
      "type": "shell",
      "command": "npm run lint:fix",
      "group": "build",
      "presentation": {
        "reveal": "silent"
      }
    },
    {
      "label": "format",
      "type": "shell",
      "command": "npm run format",
      "group": "build",
      "presentation": {
        "reveal": "silent"
      }
    }
  ]
}
```

## 🧩 Snippets Personalizzati

### JavaScript Snippets

```json
// .vscode/javascript.json
{
  "Import React": {
    "prefix": "imr",
    "body": ["import React from 'react'"],
    "description": "Import React"
  },
  "Import React with useState": {
    "prefix": "imrs",
    "body": ["import React, { useState } from 'react'"],
    "description": "Import React with useState"
  },
  "Functional Component": {
    "prefix": "fc",
    "body": [
      "const ${1:ComponentName} = () => {",
      "  return (",
      "    <div>",
      "      $0",
      "    </div>",
      "  )",
      "}",
      "",
      "export default ${1:ComponentName}"
    ],
    "description": "Functional Component"
  },
  "useState Hook": {
    "prefix": "us",
    "body": ["const [${1:state}, set${1/(.*)/${1:/capitalize}/}] = useState($2)"],
    "description": "useState Hook"
  },
  "useEffect Hook": {
    "prefix": "ue",
    "body": [
      "useEffect(() => {",
      "  $0",
      "}, [])"
    ],
    "description": "useEffect Hook"
  },
  "Console Log": {
    "prefix": "cl",
    "body": ["console.log($1)"],
    "description": "Console Log"
  },
  "Arrow Function": {
    "prefix": "af",
    "body": ["const ${1:functionName} = ($2) => {", "  $0", "}"],
    "description": "Arrow Function"
  }
}
```

## 🔗 Keybindings Personalizzati

### Keybindings.json

```json
// .vscode/keybindings.json
[
  {
    "key": "ctrl+shift+f",
    "command": "eslint.executeAutofix"
  },
  {
    "key": "ctrl+shift+p",
    "command": "prettier.forceFormatDocument"
  },
  {
    "key": "ctrl+shift+o",
    "command": "editor.action.organizeImports"
  },
  {
    "key": "ctrl+shift+r",
    "command": "workbench.action.tasks.runTask",
    "args": "npm: start"
  },
  {
    "key": "ctrl+shift+t",
    "command": "workbench.action.tasks.runTask",
    "args": "npm: test"
  }
]
```

## 🐛 Troubleshooting

### Problemi Comuni

**ESLint non funziona:**
```json
{
  "eslint.workingDirectories": ["."],
  "eslint.validate": ["javascript", "typescript"],
  "eslint.enable": true
}
```

**Prettier non formatta:**
```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "prettier.requireConfig": true
}
```

**TypeScript errors:**
```json
{
  "typescript.validate.enable": true,
  "typescript.preferences.includePackageJsonAutoImports": "on"
}
```

**Performance lente:**
```json
{
  "search.followSymlinks": false,
  "files.watcherExclude": {
    "**/node_modules/**": true
  }
}
```

Questa configurazione VS Code ottimizza l'esperienza di sviluppo JavaScript per tutti i framework utilizzati in PANDEV.
