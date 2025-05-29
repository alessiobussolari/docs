# TypeScript Style Guide

Guida di stile TypeScript completa per progetti PANDEV, estende le convenzioni JavaScript con type safety e best practices.

## 🎯 Overview

TypeScript aggiunge type safety ai progetti JavaScript PANDEV. Questa guida copre:

- **Type System**: Types, interfaces, generics
- **Framework Support**: React, Vue, Angular, Node.js
- **Tools**: TSC, ESLint, Prettier
- **Best Practices**: Type safety, performance, maintainability

## 📋 Standard Adottati

### Configurazione Base TypeScript
Utilizziamo **strict mode** per massima type safety:

```json
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

### ESLint TypeScript
Estende la configurazione JavaScript con regole TypeScript:

```json
{
  "extends": [
    "standard",
    "@typescript-eslint/recommended",
    "@typescript-eslint/recommended-requiring-type-checking",
    "prettier"
  ],
  "parser": "@typescript-eslint/parser"
}
```

## 🛠️ Setup Rapido

### 1. Installazione Dependencies

```bash
# Core TypeScript
npm install --save-dev typescript

# ESLint TypeScript
npm install --save-dev @typescript-eslint/parser
npm install --save-dev @typescript-eslint/eslint-plugin

# Standard config with TypeScript
npm install --save-dev eslint-config-standard-with-typescript

# Prettier (già configurato per JS)
npm install --save-dev prettier eslint-config-prettier
```

### 2. File tsconfig.json PANDEV

```json
{
  "compilerOptions": {
    // Strict Type Checking
    "strict": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,

    // Module Resolution
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "node",
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "resolveJsonModule": true,
    "isolatedModules": true,

    // Output
    "outDir": "./dist",
    "rootDir": "./src",
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,

    // Interop & Quality
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "exactOptionalPropertyTypes": true,
    "noUncheckedIndexedAccess": true,

    // Path Mapping
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@/components/*": ["src/components/*"],
      "@/utils/*": ["src/utils/*"],
      "@/types/*": ["src/types/*"]
    }
  },
  "include": [
    "src/**/*",
    "tests/**/*"
  ],
  "exclude": [
    "node_modules",
    "dist",
    "build"
  ]
}
```

### 3. Scripts Package.json

```json
{
  "scripts": {
    "type-check": "tsc --noEmit",
    "type-check:watch": "tsc --noEmit --watch",
    "build": "tsc",
    "lint": "eslint src/**/*.{ts,tsx}",
    "lint:fix": "eslint src/**/*.{ts,tsx} --fix"
  }
}
```

## 🎨 Framework-Specific Configurations

### React + TypeScript

```json
// tsconfig.json per React
{
  "compilerOptions": {
    "lib": ["DOM", "DOM.Iterable", "ES6"],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "include": [
    "src"
  ]
}
```

### Vue + TypeScript

```json
// tsconfig.json per Vue 3
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "strict": true,
    "jsx": "preserve",
    "moduleResolution": "node",
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "forceConsistentCasingInFileNames": true,
    "useDefineForClassFields": true,
    "sourceMap": true,
    "baseUrl": ".",
    "types": [
      "webpack-env",
      "node"
    ],
    "paths": {
      "@/*": [
        "src/*"
      ]
    },
    "lib": [
      "ES2020",
      "DOM",
      "DOM.Iterable"
    ]
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "tests/**/*.ts",
    "tests/**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}
```

### Angular + TypeScript

```json
// tsconfig.json per Angular
{
  "compileOnSave": false,
  "compilerOptions": {
    "baseUrl": "./",
    "outDir": "./dist/out-tsc",
    "forceConsistentCasingInFileNames": true,
    "strict": true,
    "noImplicitOverride": true,
    "noPropertyAccessFromIndexSignature": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "sourceMap": true,
    "declaration": false,
    "downlevelIteration": true,
    "experimentalDecorators": true,
    "moduleResolution": "node",
    "importHelpers": true,
    "target": "es2020",
    "module": "es2020",
    "lib": [
      "es2020",
      "dom"
    ]
  },
  "angularCompilerOptions": {
    "enableI18nLegacyMessageIdFormat": false,
    "strictInjectionParameters": true,
    "strictInputAccessModifiers": true,
    "strictTemplates": true
  }
}
```

### Node.js + TypeScript

```json
// tsconfig.json per Node.js
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "CommonJS",
    "lib": ["ES2022"],
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "resolveJsonModule": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true,
    "removeComments": true,
    "noImplicitReturns": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "types": ["node"]
  },
  "include": [
    "src/**/*"
  ],
  "exclude": [
    "node_modules",
    "dist"
  ]
}
```

## 📝 Type System Best Practices

### 1. Type Definitions

```typescript
// ✅ Usa interfaces per oggetti
interface User {
  readonly id: number
  name: string
  email: string
  createdAt: Date
  profile?: UserProfile
}

// ✅ Usa type aliases per unions e primitives
type Status = 'active' | 'inactive' | 'pending'
type ID = string | number

// ✅ Usa generics per riusabilità
interface ApiResponse<T> {
  data: T
  status: number
  message: string
}
```

### 2. Utility Types

```typescript
// ✅ Sfrutta utility types built-in
type CreateUser = Omit<User, 'id' | 'createdAt'>
type UpdateUser = Partial<Pick<User, 'name' | 'email'>>
type UserKeys = keyof User

// ✅ Crea utility types custom
type NonNullable<T> = T extends null | undefined ? never : T
type OptionalKeys<T> = {
  [K in keyof T]-?: {} extends Pick<T, K> ? K : never
}[keyof T]
```

### 3. Type Guards

```typescript
// ✅ Type guards per runtime checking
function isUser(obj: unknown): obj is User {
  return (
    typeof obj === 'object' &&
    obj !== null &&
    'id' in obj &&
    'name' in obj &&
    'email' in obj
  )
}

// ✅ Assertion functions
function assertIsNumber(value: unknown): asserts value is number {
  if (typeof value !== 'number') {
    throw new Error('Expected number')
  }
}
```

## 🎯 Naming Conventions

### TypeScript Specific

```typescript
// ✅ Interfaces: PascalCase
interface UserProfile {
  bio: string
  avatar: string
}

// ✅ Types: PascalCase
type ApiError = {
  code: string
  message: string
}

// ✅ Enums: PascalCase
enum UserRole {
  ADMIN = 'admin',
  USER = 'user',
  MODERATOR = 'moderator'
}

// ✅ Generics: single uppercase letter
function identity<T>(arg: T): T {
  return arg
}

// ✅ Namespace: PascalCase
namespace ApiTypes {
  export interface Request {
    url: string
  }
}
```

## 🔧 Configuration Files

### ESLint + TypeScript

```javascript
// .eslintrc.js
module.exports = {
  root: true,
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  extends: [
    'standard-with-typescript',
    'prettier'
  ],
  parserOptions: {
    project: './tsconfig.json'
  },
  rules: {
    // TypeScript specific
    '@typescript-eslint/no-unused-vars': 'error',
    '@typescript-eslint/no-explicit-any': 'warn',
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off',
    '@typescript-eslint/no-non-null-assertion': 'warn',
    '@typescript-eslint/prefer-nullish-coalescing': 'error',
    '@typescript-eslint/prefer-optional-chain': 'error',
    
    // PANDEV customs
    '@typescript-eslint/consistent-type-imports': 'error',
    '@typescript-eslint/consistent-type-definitions': ['error', 'interface'],
    '@typescript-eslint/array-type': ['error', { default: 'array-simple' }]
  }
}
```

### Prettier TypeScript

```json
{
  "semi": false,
  "singleQuote": true,
  "trailingComma": "none",
  "printWidth": 100,
  "tabWidth": 2,
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

## 🚀 Performance & Best Practices

### Compilation Performance

```json
// tsconfig.json ottimizzato
{
  "compilerOptions": {
    "incremental": true,
    "tsBuildInfoFile": "./buildcache/tsbuildinfo"
  },
  "ts-node": {
    "transpileOnly": true,
    "files": true
  }
}
```

### Type-only Imports

```typescript
// ✅ Use type-only imports quando possibile
import type { User } from './types'
import type { ComponentProps } from 'react'

// ✅ Mixed imports
import { api, type ApiResponse } from './api'
```

### Strict Configurations

```json
// tsconfig.strict.json per progetti nuovi
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "noImplicitAny": true,
    "strictNullChecks": true,
    "strictFunctionTypes": true,
    "strictBindCallApply": true,
    "strictPropertyInitialization": true,
    "noImplicitThis": true,
    "noImplicitReturns": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true
  }
}
```

## 📚 Risorse

### Documentazione Ufficiale
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [ESLint TypeScript Rules](https://typescript-eslint.io/rules/)
- [TypeScript Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

### Framework Integration
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)
- [Vue TypeScript Guide](https://vuejs.org/guide/typescript/overview.html)
- [Angular TypeScript Guide](https://angular.io/guide/typescript-configuration)

Per configurazioni dettagliate e regole specifiche, consulta le sezioni dedicate:
- [Configurazione ESLint TypeScript](configurazione-eslint.md)
- [Configurazione TSConfig](configurazione-tsconfig.md)
- [Type System Guidelines](tipi.md)
- [Performance Guidelines](performance.md)
- [Regole Specifiche](regole/README.md)
