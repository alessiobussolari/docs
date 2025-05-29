# Funzioni JavaScript

Regole per la definizione e uso delle funzioni in progetti PANDEV.

## 🎯 Function Declarations vs Expressions

### Function Declarations

**❌ Evitare:**
```javascript
// bad - function expression when declaration is better
const processUser = function(user) {
  return user.name.toUpperCase()
}

// bad - anonymous function expression
const users = data.map(function(item) {
  return item.user
})
```

**✅ Preferire:**
```javascript
// good - function declaration for top-level functions
function processUser(user) {
  return user.name.toUpperCase()
}

// good - arrow function for short callbacks
const users = data.map(item => item.user)

// good - named function expressions when needed
const processUser = function processUser(user) {
  // The name helps with debugging
  return user.name.toUpperCase()
}
```

### Arrow Functions

**❌ Evitare:**
```javascript
// bad - arrow function for object methods
const userService = {
  users: [],
  getUser: (id) => {
    return this.users.find(u => u.id === id) // 'this' is undefined
  }
}

// bad - unnecessary block for simple return
const getName = (user) => {
  return user.name
}

// bad - arrow function for constructors
const User = (name) => {
  this.name = name // TypeError: Cannot set property
}
```

**✅ Preferire:**
```javascript
// good - arrow functions for callbacks
const activeUsers = users.filter(user => user.active)
const userNames = users.map(user => user.name)

// good - implicit return for simple expressions
const getName = user => user.name
const isAdult = user => user.age >= 18

// good - regular function for object methods
const userService = {
  users: [],
  getUser(id) {
    return this.users.find(user => user.id === id)
  }
}

// good - function declaration for constructors
function User(name) {
  this.name = name
}
```

## 🔧 Function Parameters

### Parameter Defaults

**❌ Evitare:**
```javascript
// bad - checking undefined manually
function createUser(name, role) {
  if (role === undefined) {
    role = 'user'
  }
  return { name, role }
}

// bad - using || for defaults (fails with falsy values)
function calculateTotal(amount, tax) {
  tax = tax || 0.1
  return amount * (1 + tax)
}
```

**✅ Preferire:**
```javascript
// good - default parameters
function createUser(name, role = 'user') {
  return { name, role }
}

function calculateTotal(amount, tax = 0.1) {
  return amount * (1 + tax)
}

// good - complex default values
function createApiClient(config = {}) {
  const {
    baseUrl = 'https://api.example.com',
    timeout = 5000,
    retries = 3
  } = config
  
  return { baseUrl, timeout, retries }
}
```

### Destructuring Parameters

**❌ Evitare:**
```javascript
// bad - accessing object properties repeatedly
function processUser(user) {
  console.log(user.name)
  console.log(user.email)
  return user.name + ' (' + user.email + ')'
}

// bad - many individual parameters
function createNotification(title, message, type, urgent, timestamp, userId) {
  return { title, message, type, urgent, timestamp, userId }
}
```

**✅ Preferire:**
```javascript
// good - destructuring parameters
function processUser({ name, email, id }) {
  console.log(name)
  console.log(email)
  return `${name} (${email})`
}

// good - object parameter with defaults
function createNotification({
  title,
  message,
  type = 'info',
  urgent = false,
  timestamp = new Date(),
  userId
}) {
  return { title, message, type, urgent, timestamp, userId }
}

// good - array destructuring
function calculateDistance([x1, y1], [x2, y2]) {
  return Math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2)
}
```

### Rest Parameters

**❌ Evitare:**
```javascript
// bad - using arguments object
function sum() {
  let total = 0
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i]
  }
  return total
}

// bad - Array.from on arguments
function logMessages() {
  const messages = Array.from(arguments)
  messages.forEach(msg => console.log(msg))
}
```

**✅ Preferire:**
```javascript
// good - rest parameters
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0)
}

function logMessages(...messages) {
  messages.forEach(msg => console.log(msg))
}

// good - combining regular params with rest
function createUrl(baseUrl, ...pathSegments) {
  return baseUrl + '/' + pathSegments.join('/')
}
```

## 🚀 Async Functions

### Async/Await

**❌ Evitare:**
```javascript
// bad - Promise chains
function getUserData(id) {
  return fetch(`/users/${id}`)
    .then(response => response.json())
    .then(user => {
      return fetch(`/users/${id}/profile`)
        .then(response => response.json())
        .then(profile => {
          return { ...user, profile }
        })
    })
}

// bad - mixing async/await with .then()
async function processUser(id) {
  const user = await getUser(id)
  return updateUser(user).then(result => result.data)
}
```

**✅ Preferire:**
```javascript
// good - clean async/await
async function getUserData(id) {
  const userResponse = await fetch(`/users/${id}`)
  const user = await userResponse.json()
  
  const profileResponse = await fetch(`/users/${id}/profile`)
  const profile = await profileResponse.json()
  
  return { ...user, profile }
}

// good - consistent async/await
async function processUser(id) {
  const user = await getUser(id)
  const result = await updateUser(user)
  return result.data
}
```

### Error Handling in Async Functions

**❌ Evitare:**
```javascript
// bad - no error handling
async function fetchUserData(id) {
  const response = await fetch(`/users/${id}`)
  return response.json()
}

// bad - catching but not handling properly
async function saveUser(user) {
  try {
    await api.saveUser(user)
  } catch (error) {
    console.log(error) // Just logging, not handling
  }
}
```

**✅ Preferire:**
```javascript
// good - proper error handling
async function fetchUserData(id) {
  try {
    const response = await fetch(`/users/${id}`)
    
    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`)
    }
    
    return await response.json()
  } catch (error) {
    logger.error('Failed to fetch user data:', error)
    throw new UserDataError(`Unable to fetch user ${id}`)
  }
}

// good - specific error handling
async function saveUser(user) {
  try {
    const result = await api.saveUser(user)
    return result
  } catch (error) {
    if (error.code === 'VALIDATION_ERROR') {
      throw new ValidationError(error.message)
    }
    if (error.code === 'NETWORK_ERROR') {
      throw new NetworkError('Unable to save user data')
    }
    throw error // Re-throw unknown errors
  }
}
```

## 🎨 Higher-Order Functions

### Function Composition

**❌ Evitare:**
```javascript
// bad - nested function calls
const result = processResult(transformData(validateInput(rawData)))

// bad - intermediate variables everywhere
function processUserData(users) {
  const validUsers = users.filter(user => user.isValid)
  const activeUsers = validUsers.filter(user => user.active)
  const userNames = activeUsers.map(user => user.name)
  const sortedNames = userNames.sort()
  return sortedNames
}
```

**✅ Preferire:**
```javascript
// good - pipeline with clear steps
const result = [rawData]
  .map(validateInput)
  .map(transformData)
  .map(processResult)[0]

// good - functional pipeline
function processUserData(users) {
  return users
    .filter(user => user.isValid)
    .filter(user => user.active)
    .map(user => user.name)
    .sort()
}

// good - compose function for reusability
const pipe = (...fns) => (value) => fns.reduce((acc, fn) => fn(acc), value)

const processData = pipe(
  validateInput,
  transformData,
  processResult
)
```

### Currying and Partial Application

**❌ Evitare:**
```javascript
// bad - repetitive function calls
function validateEmail(email) {
  return email.includes('@') && email.includes('.')
}

function validateEmailRequired(email) {
  return email && email.includes('@') && email.includes('.')
}

function validateEmailDomain(email, domain) {
  return email.includes('@') && email.includes('.') && email.endsWith(domain)
}
```

**✅ Preferire:**
```javascript
// good - curried validation functions
const createValidator = (rules) => (value) => {
  return rules.every(rule => rule(value))
}

const isRequired = value => value != null && value !== ''
const hasAtSymbol = value => value.includes('@')
const hasDot = value => value.includes('.')
const matchesDomain = domain => value => value.endsWith(domain)

const validateEmail = createValidator([hasAtSymbol, hasDot])
const validateRequiredEmail = createValidator([isRequired, hasAtSymbol, hasDot])
const validateCompanyEmail = createValidator([
  hasAtSymbol, 
  hasDot, 
  matchesDomain('@company.com')
])
```

## 🔄 Pure Functions

### Function Purity

**❌ Evitare:**
```javascript
// bad - side effects and external dependencies
let counter = 0

function processItem(item) {
  counter++ // Side effect
  console.log(`Processing item ${counter}`) // Side effect
  return item.name.toUpperCase()
}

// bad - mutation of input
function addTimestamp(user) {
  user.timestamp = new Date() // Mutates input
  return user
}
```

**✅ Preferire:**
```javascript
// good - pure function
function processItem(item) {
  return item.name.toUpperCase()
}

// good - no mutation, returns new object
function addTimestamp(user) {
  return {
    ...user,
    timestamp: new Date()
  }
}

// good - explicit dependencies as parameters
function formatUser(user, formatter = defaultFormatter) {
  return formatter(user)
}
```

## 📊 Function Performance

### Memoization

**❌ Evitare:**
```javascript
// bad - expensive calculation on every call
function fibonacci(n) {
  if (n <= 1) return n
  return fibonacci(n - 1) + fibonacci(n - 2)
}
```

**✅ Preferire:**
```javascript
// good - memoized expensive function
function memoize(fn) {
  const cache = new Map()
  return function(...args) {
    const key = JSON.stringify(args)
    if (cache.has(key)) {
      return cache.get(key)
    }
    const result = fn.apply(this, args)
    cache.set(key, result)
    return result
  }
}

const fibonacci = memoize(function(n) {
  if (n <= 1) return n
  return fibonacci(n - 1) + fibonacci(n - 2)
})

// good - built-in memoization for simple cases
const expensiveCalculation = (() => {
  const cache = new Map()
  return (input) => {
    if (cache.has(input)) {
      return cache.get(input)
    }
    const result = performExpensiveCalculation(input)
    cache.set(input, result)
    return result
  }
})()
```

Queste regole per le funzioni garantiscono codice JavaScript più leggibile, performante e maintainable nei progetti PANDEV.
