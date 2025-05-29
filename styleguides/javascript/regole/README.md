# Regole JavaScript

Regole dettagliate di stile JavaScript per progetti PANDEV seguendo ESLint Standard e best practices moderne.

## 📋 Sezioni

### Fondamentali
- [**Generale**](generale.md) - Principi base e best practices JavaScript
- [**Sintassi**](sintassi.md) - ES6+, arrow functions, destructuring
- [**Layout**](layout.md) - Spacing, indentation, line breaks
- [**Denominazione**](denominazione.md) - camelCase, PascalCase, conventions

### Strutture del Linguaggio
- [**Funzioni**](funzioni.md) - Function declarations, async/await, closures
- [**Oggetti e Array**](oggetti-array.md) - Object/array manipulations, methods
- [**Moduli**](moduli.md) - Imports/exports, ES modules, organization

### Pattern Avanzati
- [**Asincrono**](asincrono.md) - Promises, async/await, concurrency
- [**Eccezioni**](eccezioni.md) - Error handling, try/catch, custom errors
- [**Commenti**](commenti.md) - JSDoc, inline comments, documentation

## 🎯 Principi PANDEV JavaScript

### 1. Modern JavaScript (ES6+)
```javascript
// ✅ Use const/let instead of var
const users = []
let currentUser = null

// ✅ Use arrow functions for callbacks
const activeUsers = users.filter(user => user.active)

// ✅ Use template literals
const message = `Hello ${user.name}, you have ${notifications.length} notifications`
```

### 2. Functional Programming
```javascript
// ✅ Prefer immutable operations
const newUsers = [...users, newUser]
const updatedUser = { ...user, lastLogin: new Date() }

// ✅ Use array methods over loops
const userNames = users.map(user => user.name)
const adminUsers = users.filter(user => user.role === 'admin')
```

### 3. Async/Await Pattern
```javascript
// ✅ Use async/await over Promise chains
async function fetchUserData(userId) {
  try {
    const user = await api.getUser(userId)
    const profile = await api.getUserProfile(user.id)
    return { ...user, profile }
  } catch (error) {
    logger.error('Failed to fetch user data:', error)
    throw new UserDataError('Unable to fetch user data')
  }
}
```

### 4. Error Handling
```javascript
// ✅ Specific error handling
function processPayment(amount, paymentMethod) {
  if (amount <= 0) {
    throw new ValidationError('Amount must be positive')
  }
  
  if (!paymentMethod) {
    throw new ValidationError('Payment method is required')
  }
  
  // Process payment
}
```

## 🔧 ESLint Rules Overview

### Standard Rules
- `no-var` - Use const/let instead of var
- `prefer-const` - Use const when possible
- `no-unused-vars` - No unused variables
- `no-console` - No console.log in production
- `eqeqeq` - Use === instead of ==

### PANDEV Custom Rules
- `prefer-arrow-callback` - Use arrow functions for callbacks
- `prefer-template` - Use template literals over concatenation
- `object-shorthand` - Use shorthand object properties
- `prefer-destructuring` - Use destructuring when possible

## 📚 Code Quality Metrics

### Function Complexity
- **Max lines per function**: 20
- **Max parameters**: 4
- **Cyclomatic complexity**: < 10

### File Organization
- **Max lines per file**: 300
- **One export per file** (for components/classes)
- **Group imports** by type (libraries, local)

### Naming Conventions
- **Variables/Functions**: camelCase
- **Classes/Constructors**: PascalCase
- **Constants**: UPPER_SNAKE_CASE
- **Files**: kebab-case.js

## 🚀 Performance Guidelines

### Memory Management
```javascript
// ✅ Clean up event listeners
function setupComponent() {
  const handler = () => {}
  element.addEventListener('click', handler)
  
  return () => {
    element.removeEventListener('click', handler)
  }
}
```

### Efficient Operations
```javascript
// ✅ Use Set for unique values
const uniqueIds = new Set(users.map(user => user.id))

// ✅ Use Map for key-value relationships
const userMap = new Map(users.map(user => [user.id, user]))

// ✅ Debounce expensive operations
const debouncedSearch = debounce(searchFunction, 300)
```

## 🧪 Testing Considerations

### Testable Code
```javascript
// ✅ Pure functions are easier to test
function calculateTotal(items, taxRate = 0.1) {
  const subtotal = items.reduce((sum, item) => sum + item.price, 0)
  return subtotal * (1 + taxRate)
}

// ✅ Dependency injection for mocking
function createUserService(apiClient, logger) {
  return {
    async getUser(id) {
      try {
        return await apiClient.get(`/users/${id}`)
      } catch (error) {
        logger.error('Failed to get user:', error)
        throw error
      }
    }
  }
}
```

Queste regole seguono gli standard PANDEV e sono enforce tramite ESLint e Prettier. Ogni sezione contiene esempi dettagliati con pattern ✅ good e ❌ bad per chiarezza.
