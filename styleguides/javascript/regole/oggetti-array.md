# Oggetti e Array JavaScript

Regole per l'uso efficace di oggetti e array nei progetti PANDEV.

## 🏗️ Object Creation e Manipulation

### Object Literals

**❌ Evitare:**
```javascript
// bad - using Object constructor
const user = new Object()
user.name = 'John'
user.email = 'john@example.com'

// bad - verbose property assignment
const createUser = (name, email, role) => {
  const user = {}
  user.name = name
  user.email = email
  user.role = role
  return user
}

// bad - mixed quote styles
const config = {
  'api-url': 'https://api.example.com',
  timeout: 5000,
  "max-retries": 3
}
```

**✅ Preferire:**
```javascript
// good - object literal syntax
const user = {
  name: 'John',
  email: 'john@example.com'
}

// good - property shorthand
const createUser = (name, email, role) => {
  return { name, email, role }
}

// good - consistent quotes (prefer no quotes when possible)
const config = {
  apiUrl: 'https://api.example.com',
  timeout: 5000,
  maxRetries: 3
}

// good - quotes only when necessary
const config = {
  apiUrl: 'https://api.example.com',
  'api-version': 'v1', // kebab-case requires quotes
  timeout: 5000
}
```

### Object Property Access

**❌ Evitare:**
```javascript
// bad - bracket notation when dot notation works
const userName = user['name']
const userEmail = user['email']

// bad - not checking for property existence
const avatar = user.profile.avatar // May throw error

// bad - repetitive object access
console.log(user.profile.settings.theme)
console.log(user.profile.settings.language)
console.log(user.profile.settings.timezone)
```

**✅ Preferire:**
```javascript
// good - dot notation when possible
const userName = user.name
const userEmail = user.email

// good - bracket notation for dynamic keys
const getProperty = (obj, key) => obj[key]
const fieldValue = user[fieldName]

// good - optional chaining for safe access
const avatar = user?.profile?.avatar

// good - destructuring for multiple properties
const { theme, language, timezone } = user?.profile?.settings || {}
```

### Object Copying e Updating

**❌ Evitare:**
```javascript
// bad - mutating original object
function updateUser(user, changes) {
  user.name = changes.name
  user.email = changes.email
  return user
}

// bad - Object.assign without spread
const updatedUser = Object.assign({}, user, changes)

// bad - deep mutation
user.preferences.theme = 'dark'
user.settings.notifications.email = false
```

**✅ Preferire:**
```javascript
// good - immutable updates with spread
function updateUser(user, changes) {
  return { ...user, ...changes }
}

// good - nested object updates
const updateUserPreferences = (user, newPreferences) => ({
  ...user,
  preferences: {
    ...user.preferences,
    ...newPreferences
  }
})

// good - deep updates with helper function
const updateNestedProperty = (obj, path, value) => {
  const keys = path.split('.')
  const lastKey = keys.pop()
  const nested = keys.reduce((acc, key) => ({ ...acc, [key]: { ...acc[key] } }), obj)
  
  return {
    ...nested,
    [keys.join('.')]: {
      ...nested[keys.join('.')],
      [lastKey]: value
    }
  }
}
```

## 📚 Array Operations

### Array Creation

**❌ Evitare:**
```javascript
// bad - Array constructor for literals
const numbers = new Array(1, 2, 3, 4, 5)

// bad - Array constructor ambiguity
const items = new Array(10) // Creates empty array with length 10
const items2 = new Array(10, 20) // Creates [10, 20]

// bad - manual array building
const range = []
for (let i = 0; i < 10; i++) {
  range.push(i)
}
```

**✅ Preferire:**
```javascript
// good - array literal syntax
const numbers = [1, 2, 3, 4, 5]

// good - Array.from for array creation
const range = Array.from({ length: 10 }, (_, i) => i)
const squares = Array.from({ length: 5 }, (_, i) => i * i)

// good - spread with existing arrays
const newItems = [...existingItems, newItem]
const combined = [...array1, ...array2]
```

### Array Methods

**❌ Evitare:**
```javascript
// bad - for loop for transformations
const userNames = []
for (let i = 0; i < users.length; i++) {
  userNames.push(users[i].name)
}

// bad - for loop for filtering
const activeUsers = []
for (let i = 0; i < users.length; i++) {
  if (users[i].active) {
    activeUsers.push(users[i])
  }
}

// bad - mutating array methods when immutability is desired
const sortedUsers = users.sort((a, b) => a.name.localeCompare(b.name))
```

**✅ Preferire:**
```javascript
// good - array methods for transformations
const userNames = users.map(user => user.name)

// good - filtering with array methods
const activeUsers = users.filter(user => user.active)

// good - method chaining
const activeUserNames = users
  .filter(user => user.active)
  .map(user => user.name)
  .sort()

// good - non-mutating sort
const sortedUsers = [...users].sort((a, b) => a.name.localeCompare(b.name))
```

### Array Search e Testing

**❌ Evitare:**
```javascript
// bad - indexOf for existence check
if (permissions.indexOf('admin') !== -1) {
  // user is admin
}

// bad - for loop for finding items
let foundUser = null
for (let i = 0; i < users.length; i++) {
  if (users[i].id === userId) {
    foundUser = users[i]
    break
  }
}

// bad - manual array testing
let hasActiveUser = false
for (const user of users) {
  if (user.active) {
    hasActiveUser = true
    break
  }
}
```

**✅ Preferire:**
```javascript
// good - includes for existence check
if (permissions.includes('admin')) {
  // user is admin
}

// good - find for single item
const foundUser = users.find(user => user.id === userId)

// good - some/every for testing
const hasActiveUser = users.some(user => user.active)
const allUsersActive = users.every(user => user.active)

// good - findIndex for index-based operations
const userIndex = users.findIndex(user => user.id === userId)
```

### Array Aggregation

**❌ Evitare:**
```javascript
// bad - manual accumulation
let total = 0
for (const item of items) {
  total += item.price
}

// bad - multiple passes for complex aggregations
const prices = items.map(item => item.price)
const total = prices.reduce((sum, price) => sum + price, 0)
const average = total / prices.length
```

**✅ Preferire:**
```javascript
// good - reduce for aggregation
const total = items.reduce((sum, item) => sum + item.price, 0)

// good - single pass aggregation
const { total, count } = items.reduce(
  (acc, item) => ({
    total: acc.total + item.price,
    count: acc.count + 1
  }),
  { total: 0, count: 0 }
)
const average = count > 0 ? total / count : 0

// good - groupBy implementation
const groupBy = (array, keyFn) => {
  return array.reduce((groups, item) => {
    const key = keyFn(item)
    return {
      ...groups,
      [key]: [...(groups[key] || []), item]
    }
  }, {})
}

const usersByRole = groupBy(users, user => user.role)
```

## 🔄 Advanced Patterns

### Object Destructuring Patterns

**❌ Evitare:**
```javascript
// bad - repetitive property access
function processUser(user) {
  console.log(user.name)
  validateEmail(user.email)
  checkPermissions(user.role)
  return user.id
}

// bad - not using default values
function createConfig(options) {
  const timeout = options.timeout || 5000
  const retries = options.retries || 3
  const baseUrl = options.baseUrl || 'https://api.example.com'
  return { timeout, retries, baseUrl }
}
```

**✅ Preferire:**
```javascript
// good - destructuring with defaults
function processUser({ name, email, role, id }) {
  console.log(name)
  validateEmail(email)
  checkPermissions(role)
  return id
}

// good - parameter destructuring with defaults
function createConfig({
  timeout = 5000,
  retries = 3,
  baseUrl = 'https://api.example.com'
} = {}) {
  return { timeout, retries, baseUrl }
}

// good - nested destructuring
function processResponse({ data: { users, pagination } }) {
  return {
    users: users.map(user => user.name),
    totalPages: pagination.totalPages
  }
}
```

### Array Destructuring Patterns

**❌ Evitare:**
```javascript
// bad - array index access
const first = items[0]
const second = items[1]
const rest = items.slice(2)

// bad - swapping with temporary variable
let temp = a
a = b
b = temp
```

**✅ Preferire:**
```javascript
// good - array destructuring
const [first, second, ...rest] = items

// good - swapping with destructuring
[a, b] = [b, a]

// good - ignoring elements
const [, , third] = items // Skip first two elements

// good - with default values
const [x = 0, y = 0] = coordinates || []
```

### Dynamic Object Manipulation

**❌ Evitare:**
```javascript
// bad - manual object building
function createFilteredObject(obj, allowedKeys) {
  const result = {}
  for (const key of allowedKeys) {
    if (obj.hasOwnProperty(key)) {
      result[key] = obj[key]
    }
  }
  return result
}

// bad - conditional object properties
const user = { name: userName, email: userEmail }
if (userAge) {
  user.age = userAge
}
if (userRole) {
  user.role = userRole
}
```

**✅ Preferire:**
```javascript
// good - object manipulation with modern methods
const createFilteredObject = (obj, allowedKeys) => {
  return Object.fromEntries(
    Object.entries(obj).filter(([key]) => allowedKeys.includes(key))
  )
}

// good - conditional properties with spread
const user = {
  name: userName,
  email: userEmail,
  ...(userAge && { age: userAge }),
  ...(userRole && { role: userRole })
}

// good - computed property names
const createDynamicObject = (key, value) => ({
  [key]: value,
  [`${key}_timestamp`]: Date.now()
})
```

## 🎯 Performance Considerations

### Efficient Operations

**❌ Evitare:**
```javascript
// bad - inefficient array search
const hasPermission = (permissions, required) => {
  return permissions.filter(p => required.includes(p)).length > 0
}

// bad - repeated array operations
function processItems(items) {
  const valid = items.filter(item => item.isValid)
  const active = items.filter(item => item.isActive)
  const premium = items.filter(item => item.isPremium)
  return { valid, active, premium }
}
```

**✅ Preferire:**
```javascript
// good - efficient search with some
const hasPermission = (permissions, required) => {
  return permissions.some(p => required.includes(p))
}

// good - single pass with reduce
function processItems(items) {
  return items.reduce(
    (acc, item) => ({
      valid: item.isValid ? [...acc.valid, item] : acc.valid,
      active: item.isActive ? [...acc.active, item] : acc.active,
      premium: item.isPremium ? [...acc.premium, item] : acc.premium
    }),
    { valid: [], active: [], premium: [] }
  )
}

// good - Set for fast lookups
const requiredSet = new Set(required)
const hasPermission = permissions => permissions.some(p => requiredSet.has(p))
```

Queste regole garantiscono uso efficiente e maintainable di oggetti e array nei progetti JavaScript PANDEV.
