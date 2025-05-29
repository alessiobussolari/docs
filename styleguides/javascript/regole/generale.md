# Generale JavaScript

Principi fondamentali e best practices per lo sviluppo JavaScript nei progetti PANDEV.

## 🎯 Principi Fondamentali

### Code Quality Principles

**❌ Evitare:**
```javascript
// bad - unclear intent
function calc(a, b, c) {
  return a + b * c
}

// bad - magic numbers
setTimeout(() => {
  checkStatus()
}, 30000)

// bad - deep nesting
function processData(data) {
  if (data) {
    if (data.users) {
      if (data.users.length > 0) {
        data.users.forEach(user => {
          if (user.active) {
            if (user.permissions) {
              // process user
            }
          }
        })
      }
    }
  }
}
```

**✅ Preferire:**
```javascript
// good - clear intent and documentation
/**
 * Calculate total price including tax
 * @param {number} basePrice - Base price before tax
 * @param {number} taxRate - Tax rate as decimal (e.g., 0.1 for 10%)
 * @param {number} quantity - Number of items
 * @returns {number} Total price including tax
 */
function calculateTotalPrice(basePrice, taxRate, quantity) {
  return basePrice * quantity * (1 + taxRate)
}

// good - named constants
const STATUS_CHECK_INTERVAL = 30000 // 30 seconds

setTimeout(() => {
  checkStatus()
}, STATUS_CHECK_INTERVAL)

// good - early returns and guard clauses
function processData(data) {
  if (!data?.users?.length) {
    return
  }
  
  const activeUsers = data.users.filter(user => user.active)
  const usersWithPermissions = activeUsers.filter(user => user.permissions)
  
  usersWithPermissions.forEach(processUser)
}
```

### DRY (Don't Repeat Yourself)

**❌ Evitare:**
```javascript
// bad - repeated validation logic
function validateUser(user) {
  if (!user.name || user.name.length < 2) {
    return false
  }
  if (!user.email || !user.email.includes('@')) {
    return false
  }
  return true
}

function validateAdmin(admin) {
  if (!admin.name || admin.name.length < 2) {
    return false
  }
  if (!admin.email || !admin.email.includes('@')) {
    return false
  }
  if (!admin.role || admin.role !== 'admin') {
    return false
  }
  return true
}
```

**✅ Preferire:**
```javascript
// good - reusable validation functions
const validators = {
  name: name => name && name.length >= 2,
  email: email => email && email.includes('@'),
  role: (role, expected) => role === expected
}

function validateUser(user) {
  return validators.name(user.name) && validators.email(user.email)
}

function validateAdmin(admin) {
  return validateUser(admin) && validators.role(admin.role, 'admin')
}

// good - configurable validation
function createValidator(rules) {
  return (data) => rules.every(rule => rule(data))
}

const userValidator = createValidator([
  user => validators.name(user.name),
  user => validators.email(user.email)
])

const adminValidator = createValidator([
  user => validators.name(user.name),
  user => validators.email(user.email),
  user => validators.role(user.role, 'admin')
])
```

## 🔧 SOLID Principles in JavaScript

### Single Responsibility Principle

**❌ Evitare:**
```javascript
// bad - class doing too many things
class UserManager {
  constructor() {
    this.users = []
  }
  
  addUser(user) {
    this.users.push(user)
  }
  
  removeUser(userId) {
    this.users = this.users.filter(u => u.id !== userId)
  }
  
  sendEmail(userId, message) {
    // email sending logic
  }
  
  validateUser(user) {
    // validation logic
  }
  
  saveToDatabase(user) {
    // database logic
  }
}
```

**✅ Preferire:**
```javascript
// good - separate responsibilities
class UserRepository {
  constructor() {
    this.users = []
  }
  
  add(user) {
    this.users.push(user)
  }
  
  remove(userId) {
    this.users = this.users.filter(u => u.id !== userId)
  }
  
  findById(userId) {
    return this.users.find(u => u.id === userId)
  }
}

class UserValidator {
  static validate(user) {
    return user.name && user.email && user.email.includes('@')
  }
}

class EmailService {
  static send(to, message) {
    // email sending logic
  }
}

class DatabaseService {
  static save(collection, data) {
    // database logic
  }
}
```

### Dependency Inversion

**❌ Evitare:**
```javascript
// bad - tight coupling to specific implementations
class UserService {
  constructor() {
    this.emailService = new EmailService()
    this.database = new MySQLDatabase()
  }
  
  createUser(userData) {
    const user = new User(userData)
    this.database.save(user)
    this.emailService.sendWelcome(user.email)
    return user
  }
}
```

**✅ Preferire:**
```javascript
// good - dependency injection
class UserService {
  constructor(emailService, database) {
    this.emailService = emailService
    this.database = database
  }
  
  createUser(userData) {
    const user = new User(userData)
    this.database.save(user)
    this.emailService.sendWelcome(user.email)
    return user
  }
}

// Usage
const emailService = new EmailService()
const database = new PostgreSQLDatabase()
const userService = new UserService(emailService, database)
```

## 📦 Module Design Patterns

### Module Pattern

**❌ Evitare:**
```javascript
// bad - global variables
var userName = 'default'
var userSettings = {}

function updateUserName(name) {
  userName = name
}

function getUserName() {
  return userName
}
```

**✅ Preferire:**
```javascript
// good - module pattern
const UserModule = (function() {
  let userName = 'default'
  let userSettings = {}
  
  return {
    updateUserName(name) {
      userName = name
    },
    
    getUserName() {
      return userName
    },
    
    updateSettings(settings) {
      userSettings = { ...userSettings, ...settings }
    }
  }
})()

// good - ES6 modules
class UserManager {
  #userName = 'default'
  #userSettings = {}
  
  updateUserName(name) {
    this.#userName = name
  }
  
  getUserName() {
    return this.#userName
  }
}

export default new UserManager()
```

### Factory Pattern

**❌ Evitare:**
```javascript
// bad - direct instantiation everywhere
const adminUser = new User('admin', 'admin@example.com', 'admin')
const regularUser = new User('john', 'john@example.com', 'user')
const guestUser = new User('guest', 'guest@example.com', 'guest')
```

**✅ Preferire:**
```javascript
// good - factory pattern
class UserFactory {
  static createAdmin(name, email) {
    return new User(name, email, 'admin', {
      canManageUsers: true,
      canAccessAdmin: true,
      maxPermissions: 'all'
    })
  }
  
  static createRegularUser(name, email) {
    return new User(name, email, 'user', {
      canManageUsers: false,
      canAccessAdmin: false,
      maxPermissions: 'limited'
    })
  }
  
  static createGuest() {
    return new User('Guest', 'guest@example.com', 'guest', {
      canManageUsers: false,
      canAccessAdmin: false,
      maxPermissions: 'read-only'
    })
  }
}

// Usage
const admin = UserFactory.createAdmin('John Doe', 'john@example.com')
const user = UserFactory.createRegularUser('Jane Smith', 'jane@example.com')
const guest = UserFactory.createGuest()
```

## 🎨 Functional Programming Principles

### Immutability

**❌ Evitare:**
```javascript
// bad - mutating objects
function updateUser(user, changes) {
  user.name = changes.name
  user.email = changes.email
  user.updatedAt = new Date()
  return user
}

// bad - mutating arrays
function addNotification(notifications, newNotification) {
  notifications.push(newNotification)
  return notifications
}
```

**✅ Preferire:**
```javascript
// good - immutable updates
function updateUser(user, changes) {
  return {
    ...user,
    ...changes,
    updatedAt: new Date()
  }
}

// good - immutable array operations
function addNotification(notifications, newNotification) {
  return [...notifications, newNotification]
}

// good - immutable helpers
const immutableHelpers = {
  update: (obj, changes) => ({ ...obj, ...changes }),
  
  updateNested: (obj, path, value) => {
    const keys = path.split('.')
    const [head, ...tail] = keys
    
    if (tail.length === 0) {
      return { ...obj, [head]: value }
    }
    
    return {
      ...obj,
      [head]: immutableHelpers.updateNested(obj[head] || {}, tail.join('.'), value)
    }
  }
}
```

### Pure Functions

**❌ Evitare:**
```javascript
// bad - side effects
let requestCount = 0

function makeRequest(url) {
  requestCount++ // side effect
  console.log(`Making request ${requestCount}`) // side effect
  return fetch(url)
}

// bad - depending on external state
function formatPrice(amount) {
  return `${CURRENCY_SYMBOL}${amount.toFixed(2)}` // depends on global
}
```

**✅ Preferire:**
```javascript
// good - pure functions
function formatPrice(amount, currencySymbol = '$') {
  return `${currencySymbol}${amount.toFixed(2)}`
}

function calculateTax(amount, rate) {
  return amount * rate
}

function calculateTotal(items, taxRate) {
  const subtotal = items.reduce((sum, item) => sum + item.price, 0)
  const tax = calculateTax(subtotal, taxRate)
  return subtotal + tax
}

// good - separating pure and impure functions
const pureCalculations = {
  formatPrice: (amount, symbol = '$') => `${symbol}${amount.toFixed(2)}`,
  calculateTax: (amount, rate) => amount * rate,
  calculateDiscount: (amount, percentage) => amount * (percentage / 100)
}

const sideEffects = {
  logRequest: (url) => console.log(`Making request to ${url}`),
  trackEvent: (event) => analytics.track(event),
  saveToStorage: (key, value) => localStorage.setItem(key, value)
}
```

## 🔍 Testing Considerations

### Testable Code

**❌ Evitare:**
```javascript
// bad - hard to test
function processPayment() {
  const amount = document.getElementById('amount').value
  const method = document.getElementById('method').value
  
  if (amount && method) {
    fetch('/api/payments', {
      method: 'POST',
      body: JSON.stringify({ amount, method })
    }).then(response => {
      if (response.ok) {
        document.getElementById('success').style.display = 'block'
      } else {
        document.getElementById('error').style.display = 'block'
      }
    })
  }
}
```

**✅ Preferire:**
```javascript
// good - testable separation of concerns
function extractPaymentData(formElement) {
  return {
    amount: formElement.querySelector('#amount').value,
    method: formElement.querySelector('#method').value
  }
}

function validatePaymentData({ amount, method }) {
  return {
    isValid: !!(amount && method),
    errors: {
      amount: !amount ? 'Amount is required' : null,
      method: !method ? 'Payment method is required' : null
    }
  }
}

async function submitPayment(paymentData, apiClient) {
  return await apiClient.post('/api/payments', paymentData)
}

function displayResult(success, errorElement, successElement) {
  if (success) {
    successElement.style.display = 'block'
    errorElement.style.display = 'none'
  } else {
    errorElement.style.display = 'block'
    successElement.style.display = 'none'
  }
}

// Orchestrator function
async function processPayment(formElement, apiClient, uiElements) {
  const paymentData = extractPaymentData(formElement)
  const validation = validatePaymentData(paymentData)
  
  if (!validation.isValid) {
    displayResult(false, uiElements.error, uiElements.success)
    return
  }
  
  try {
    const response = await submitPayment(paymentData, apiClient)
    displayResult(response.ok, uiElements.error, uiElements.success)
  } catch (error) {
    displayResult(false, uiElements.error, uiElements.success)
  }
}
```

Questi principi garantiscono codice JavaScript maintainable, testabile e robusto nei progetti PANDEV.
