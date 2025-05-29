# Sintassi JavaScript

Regole di sintassi JavaScript moderne seguendo ES6+ e best practices PANDEV.

## 🚀 ES6+ Features

### Dichiarazioni di Variabili

**❌ Evitare:**
```javascript
// bad - var has function scope and hoisting issues
var user = 'John'
var users = []

// bad - no re-assignment but used let
let API_URL = 'https://api.example.com'

// bad - re-assigning const
const currentUser = null
currentUser = user // TypeError
```

**✅ Preferire:**
```javascript
// good - use const by default
const API_URL = 'https://api.example.com'
const users = []

// good - use let only when reassignment is needed
let currentUser = null
let retryCount = 0

// good - const with objects/arrays (contents can change)
const user = {
  name: 'John',
  age: 30
}
user.age = 31 // OK - object contents can change
```

### Arrow Functions

**❌ Evitare:**
```javascript
// bad - verbose function syntax for simple callbacks
users.map(function(user) {
  return user.name
})

// bad - arrow function with single statement block
const getName = (user) => {
  return user.name
}

// bad - unnecessary parentheses around single parameter
const getAge = (user) => user.age

// bad - arrow function for object methods
const userService = {
  users: [],
  getUser: (id) => {
    return this.users.find(u => u.id === id) // 'this' is undefined
  }
}
```

**✅ Preferire:**
```javascript
// good - concise arrow functions for callbacks
const userNames = users.map(user => user.name)
const activeUsers = users.filter(user => user.active)

// good - implicit return for single expressions
const getName = user => user.name
const isAdmin = user => user.role === 'admin'

// good - explicit return for complex logic
const processUser = user => {
  const processed = validateUser(user)
  logUserAction(user.id, 'processed')
  return processed
}

// good - regular function for object methods (to preserve 'this')
const userService = {
  users: [],
  getUser(id) {
    return this.users.find(user => user.id === id)
  }
}
```

### Template Literals

**❌ Evitare:**
```javascript
// bad - string concatenation
const message = 'Hello ' + user.name + ', you have ' + count + ' notifications'

// bad - escaping quotes
const html = '<div class="user-card">' +
  '<h3>' + user.name + '</h3>' +
  '<p>' + user.email + '</p>' +
  '</div>'

// bad - multiline strings with concatenation
const query = 'SELECT * FROM users ' +
              'WHERE active = true ' +
              'AND role = "admin"'
```

**✅ Preferire:**
```javascript
// good - template literals for interpolation
const message = `Hello ${user.name}, you have ${count} notifications`

// good - template literals for complex strings
const html = `
  <div class="user-card">
    <h3>${user.name}</h3>
    <p>${user.email}</p>
  </div>
`

// good - multiline template literals
const query = `
  SELECT * FROM users 
  WHERE active = true 
  AND role = "admin"
`

// good - expression in template literals
const status = `User ${user.name} is ${user.active ? 'active' : 'inactive'}`
```

### Destructuring

**❌ Evitare:**
```javascript
// bad - accessing object properties multiple times
function processUser(user) {
  console.log(user.name)
  console.log(user.email)
  console.log(user.role)
  return user.name + ' (' + user.email + ')'
}

// bad - accessing array elements by index
const first = items[0]
const second = items[1]
const rest = items.slice(2)

// bad - not using parameter destructuring
function createUser(options) {
  return {
    name: options.name,
    email: options.email,
    role: options.role || 'user'
  }
}
```

**✅ Preferire:**
```javascript
// good - object destructuring
function processUser(user) {
  const { name, email, role } = user
  console.log(name)
  console.log(email)
  console.log(role)
  return `${name} (${email})`
}

// good - array destructuring
const [first, second, ...rest] = items

// good - parameter destructuring with defaults
function createUser({ name, email, role = 'user' }) {
  return { name, email, role }
}

// good - nested destructuring
const { user: { profile: { avatar } } } = response

// good - aliasing in destructuring
const { name: userName, id: userId } = user
```

### Spread Operator

**❌ Evitare:**
```javascript
// bad - mutating arrays
const originalArray = [1, 2, 3]
originalArray.push(4) // mutates original

// bad - mutating objects
const originalUser = { name: 'John', age: 30 }
originalUser.age = 31 // mutates original

// bad - concatenating arrays with concat
const combined = arr1.concat(arr2).concat(arr3)

// bad - copying objects with Object.assign
const updated = Object.assign({}, user, { lastLogin: new Date() })
```

**✅ Preferire:**
```javascript
// good - immutable array operations
const originalArray = [1, 2, 3]
const newArray = [...originalArray, 4]

// good - immutable object updates
const originalUser = { name: 'John', age: 30 }
const updatedUser = { ...originalUser, age: 31 }

// good - array concatenation with spread
const combined = [...arr1, ...arr2, ...arr3]

// good - copying and updating objects
const updated = {
  ...user,
  lastLogin: new Date(),
  preferences: { ...user.preferences, theme: 'dark' }
}

// good - function arguments with spread
function logUsers(...users) {
  users.forEach(user => console.log(user.name))
}
```

## 🔧 Control Flow

### Conditional Statements

**❌ Evitare:**
```javascript
// bad - loose equality
if (user.id == userId) {
  // handle user
}

// bad - unnecessary ternary
const isActive = user.active ? true : false

// bad - nested ternary
const status = user.active ? (user.verified ? 'verified' : 'unverified') : 'inactive'

// bad - long if-else chains
if (type === 'user') {
  handleUser()
} else if (type === 'admin') {
  handleAdmin()
} else if (type === 'moderator') {
  handleModerator()
} else {
  handleGuest()
}
```

**✅ Preferire:**
```javascript
// good - strict equality
if (user.id === userId) {
  // handle user
}

// good - direct boolean usage
const isActive = user.active

// good - clear conditional logic
let status
if (!user.active) {
  status = 'inactive'
} else if (user.verified) {
  status = 'verified'
} else {
  status = 'unverified'
}

// good - switch statement for multiple conditions
switch (type) {
  case 'user':
    handleUser()
    break
  case 'admin':
    handleAdmin()
    break
  case 'moderator':
    handleModerator()
    break
  default:
    handleGuest()
}

// good - object mapping for simple cases
const handlers = {
  user: handleUser,
  admin: handleAdmin,
  moderator: handleModerator
}
const handler = handlers[type] || handleGuest
handler()
```

### Loops and Iteration

**❌ Evitare:**
```javascript
// bad - traditional for loop for array processing
const results = []
for (let i = 0; i < users.length; i++) {
  if (users[i].active) {
    results.push(users[i].name)
  }
}

// bad - for...in for arrays
const names = []
for (const index in users) {
  names.push(users[index].name)
}

// bad - forEach for transformations
const names = []
users.forEach(user => {
  names.push(user.name)
})
```

**✅ Preferire:**
```javascript
// good - array methods for transformations
const activeUserNames = users
  .filter(user => user.active)
  .map(user => user.name)

// good - for...of for simple iteration
for (const user of users) {
  console.log(user.name)
}

// good - forEach for side effects only
users.forEach(user => {
  logUserAction(user.id, 'viewed')
})

// good - reduce for accumulations
const totalAge = users.reduce((sum, user) => sum + user.age, 0)

// good - find for single item
const adminUser = users.find(user => user.role === 'admin')
```

## 🎯 Modern Syntax Features

### Optional Chaining

**❌ Evitare:**
```javascript
// bad - manual null/undefined checking
if (user && user.profile && user.profile.address && user.profile.address.city) {
  console.log(user.profile.address.city)
}

// bad - multiple null checks
const city = user.profile ? 
  (user.profile.address ? user.profile.address.city : null) : 
  null
```

**✅ Preferire:**
```javascript
// good - optional chaining
console.log(user?.profile?.address?.city)

// good - with fallback
const city = user?.profile?.address?.city ?? 'Unknown'

// good - method calls
user?.updateProfile?.()

// good - array access
const firstItem = items?.[0]
```

### Nullish Coalescing

**❌ Evitare:**
```javascript
// bad - logical OR with falsy values
const name = user.name || 'Anonymous' // fails if name is ""
const count = user.count || 0 // fails if count is 0
const enabled = user.enabled || false // fails if enabled is false
```

**✅ Preferire:**
```javascript
// good - nullish coalescing for null/undefined only
const name = user.name ?? 'Anonymous'
const count = user.count ?? 0
const enabled = user.enabled ?? false

// good - combined with optional chaining
const theme = user?.preferences?.theme ?? 'light'
```

### Object Property Shorthand

**❌ Evitare:**
```javascript
// bad - explicit property names
const user = {
  name: name,
  email: email,
  age: age
}

// bad - explicit method definition
const api = {
  getUser: function(id) {
    return fetch(`/users/${id}`)
  }
}
```

**✅ Preferire:**
```javascript
// good - property shorthand
const user = { name, email, age }

// good - method shorthand
const api = {
  getUser(id) {
    return fetch(`/users/${id}`)
  },
  
  async getUserAsync(id) {
    const response = await fetch(`/users/${id}`)
    return response.json()
  }
}

// good - computed property names
const dynamicKey = 'userId'
const data = {
  [dynamicKey]: 123,
  [`${prefix}_name`]: user.name
}
```

## 🔄 Best Practices

### Function Declarations

**❌ Evitare:**
```javascript
// bad - function expression when declaration is better
const myFunction = function() {
  // implementation
}

// bad - arrow function for top-level functions
const processUsers = () => {
  // complex logic
}
```

**✅ Preferire:**
```javascript
// good - function declaration for top-level functions
function processUsers() {
  // complex logic with proper hoisting
}

// good - arrow functions for callbacks and short functions
const userProcessor = {
  process: users => users.map(user => ({ ...user, processed: true }))
}

// good - consistent function style in objects
const userService = {
  async getUser(id) {
    return await api.get(`/users/${id}`)
  },
  
  validateUser(user) {
    return user.email && user.name
  }
}
```

### Return Statements

**❌ Evitare:**
```javascript
// bad - unnecessary else after return
function getUserStatus(user) {
  if (!user.active) {
    return 'inactive'
  } else {
    return 'active'
  }
}

// bad - multiple return points without early returns
function processUser(user) {
  let result
  if (user.valid) {
    if (user.active) {
      result = processActiveUser(user)
    } else {
      result = processInactiveUser(user)
    }
  } else {
    result = null
  }
  return result
}
```

**✅ Preferire:**
```javascript
// good - early returns
function getUserStatus(user) {
  if (!user.active) {
    return 'inactive'
  }
  return 'active'
}

// good - guard clauses
function processUser(user) {
  if (!user.valid) {
    return null
  }
  
  if (!user.active) {
    return processInactiveUser(user)
  }
  
  return processActiveUser(user)
}
```

Queste regole di sintassi garantiscono codice JavaScript moderno, leggibile e performante seguendo gli standard PANDEV.
