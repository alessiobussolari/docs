# Layout JavaScript

Regole di formattazione e organizzazione del codice JavaScript per progetti PANDEV.

## 🎨 Spacing e Indentation

### Indentation

**❌ Evitare:**
```javascript
// bad - mixed tabs and spaces
function processUser(user) {
	const name = user.name
    const email = user.email // spaces instead of tabs
    return { name, email }
}

// bad - inconsistent indentation
if (condition) {
  doSomething()
    doSomethingElse() // wrong indentation
}
```

**✅ Preferire:**
```javascript
// good - consistent 2-space indentation
function processUser(user) {
  const name = user.name
  const email = user.email
  return { name, email }
}

// good - consistent indentation in blocks
if (condition) {
  doSomething()
  doSomethingElse()
}
```

### Spacing Around Operators

**❌ Evitare:**
```javascript
// bad - no spaces around operators
const total=price+tax
const isValid=name&&email
const result=count>0?'many':'none'

// bad - inconsistent spacing
const obj = {name:user.name,email:user.email}
```

**✅ Preferire:**
```javascript
// good - spaces around operators
const total = price + tax
const isValid = name && email
const result = count > 0 ? 'many' : 'none'

// good - consistent object spacing
const obj = { name: user.name, email: user.email }
```

### Function Calls e Declarations

**❌ Evitare:**
```javascript
// bad - space before parentheses in calls
getUserData (userId)
processUser (user)

// bad - no space after commas
function createUser(name,email,role) {
  return {name,email,role}
}

// bad - inconsistent parameter spacing
const result = calculate( amount , tax, discount )
```

**✅ Preferire:**
```javascript
// good - no space before parentheses in calls
getUserData(userId)
processUser(user)

// good - space after commas
function createUser(name, email, role) {
  return { name, email, role }
}

// good - consistent parameter spacing
const result = calculate(amount, tax, discount)
```

## 🔧 Code Organization

### File Structure

**❌ Evitare:**
```javascript
// bad - mixed imports and code
import userService from './user-service'
const DEFAULT_ROLE = 'user'
import emailService from './email-service'

function createUser() {}
import validationUtils from './validation'

export { createUser }
```

**✅ Preferire:**
```javascript
// good - organized file structure
// 1. Imports (external first, then internal)
import React from 'react'
import lodash from 'lodash'

import userService from './user-service'
import emailService from './email-service'
import validationUtils from './validation'

// 2. Constants
const DEFAULT_ROLE = 'user'
const MAX_RETRIES = 3

// 3. Helper functions
function validateUser(user) {
  return validationUtils.validate(user)
}

// 4. Main functions/classes
function createUser(userData) {
  // implementation
}

// 5. Exports
export { createUser }
export default userService
```

### Import Organization

**❌ Evitare:**
```javascript
// bad - mixed import types
import './styles.css'
import userService from './user-service'
import React from 'react'
import lodash from 'lodash'
import { validateEmail } from './validation'
```

**✅ Preferire:**
```javascript
// good - grouped imports
// External libraries
import React from 'react'
import lodash from 'lodash'
import axios from 'axios'

// Internal modules
import userService from './user-service'
import { validateEmail, validatePassword } from './validation'
import { formatCurrency } from './formatting'

// Styles and assets
import './styles.css'
import './component.scss'
```

### Function Organization

**❌ Evitare:**
```javascript
// bad - mixed function definitions
const helper1 = () => {}
function mainFunction() {
  return helper2() + helper1()
}
const helper2 = () => {}

// bad - no logical grouping
function validateUser() {}
function processPayment() {}
function validateEmail() {}
function processUser() {}
```

**✅ Preferire:**
```javascript
// good - helper functions before main functions
const validateEmail = email => /\S+@\S+\.\S+/.test(email)
const validatePassword = password => password.length >= 8
const formatUser = user => ({ ...user, name: user.name.trim() })

function validateUser(user) {
  return validateEmail(user.email) && validatePassword(user.password)
}

function processUser(userData) {
  const user = formatUser(userData)
  if (!validateUser(user)) {
    throw new Error('Invalid user data')
  }
  return user
}

// good - logical grouping
// Validation functions
function validateUser() {}
function validateEmail() {}

// Processing functions  
function processUser() {}
function processPayment() {}
```

## 📐 Line Length e Breaking

### Line Length

**❌ Evitare:**
```javascript
// bad - line too long
const message = `Hello ${user.name}, you have ${notifications.length} unread notifications and ${messages.length} new messages waiting for your attention`

// bad - poor line breaking
const result = someVeryLongFunctionName(parameter1, parameter2, parameter3, parameter4, parameter5)
```

**✅ Preferire:**
```javascript
// good - reasonable line length (max 100 chars)
const message = `Hello ${user.name}, you have ${notifications.length} ` +
  `unread notifications and ${messages.length} new messages`

// good - proper line breaking
const result = someVeryLongFunctionName(
  parameter1,
  parameter2, 
  parameter3,
  parameter4,
  parameter5
)
```

### Object e Array Breaking

**❌ Evitare:**
```javascript
// bad - inconsistent breaking
const user = { name: 'John', email: 'john@example.com',
  role: 'admin', active: true }

// bad - poor array formatting
const permissions = ['read', 'write', 'delete',
'admin', 'moderate']
```

**✅ Preferire:**
```javascript
// good - consistent object formatting
const user = {
  name: 'John',
  email: 'john@example.com',
  role: 'admin',
  active: true
}

// good - consistent array formatting
const permissions = [
  'read',
  'write', 
  'delete',
  'admin',
  'moderate'
]

// good - single line for short arrays
const colors = ['red', 'green', 'blue']
```

### Method Chaining

**❌ Evitare:**
```javascript
// bad - long chain on single line
const result = users.filter(user => user.active).map(user => user.name).sort().join(', ')

// bad - inconsistent chain breaking
const processed = data
.filter(item => item.valid).map(item => item.value)
  .reduce((acc, val) => acc + val, 0)
```

**✅ Preferire:**
```javascript
// good - proper chain breaking
const result = users
  .filter(user => user.active)
  .map(user => user.name)
  .sort()
  .join(', ')

// good - consistent indentation
const processed = data
  .filter(item => item.valid)
  .map(item => item.value)
  .reduce((acc, val) => acc + val, 0)
```

## 🗂️ Block Organization

### Conditional Blocks

**❌ Evitare:**
```javascript
// bad - no spacing around blocks
if(condition){
doSomething()
}else{
doSomethingElse()
}

// bad - single line blocks without braces
if (condition) return early
```

**✅ Preferire:**
```javascript
// good - proper spacing around blocks
if (condition) {
  doSomething()
} else {
  doSomethingElse()
}

// good - braces for single line blocks
if (condition) {
  return early
}

// good - guard clauses without braces (when very short)
if (!user) return null
if (!user.active) return null
```

### Function Blocks

**❌ Evitare:**
```javascript
// bad - cramped function definition
function processUser(user){
const name = user.name
const email = user.email
return {name, email}
}

// bad - no separation between functions
function functionOne() {
  // implementation
}
function functionTwo() {
  // implementation  
}
```

**✅ Preferire:**
```javascript
// good - proper spacing in functions
function processUser(user) {
  const name = user.name
  const email = user.email
  return { name, email }
}

// good - blank lines between functions
function functionOne() {
  // implementation
}

function functionTwo() {
  // implementation
}
```

## 📝 Comments e Spacing

### Comment Spacing

**❌ Evitare:**
```javascript
// bad - no space after //
//This is a comment
const value = 42//Another comment

// bad - inconsistent comment spacing
const user = {
  name: 'John',//name
  email: 'john@example.com', // email
  role:'admin'//role
}
```

**✅ Preferire:**
```javascript
// good - space after //
// This is a comment
const value = 42 // Another comment

// good - consistent comment spacing
const user = {
  name: 'John', // name
  email: 'john@example.com', // email
  role: 'admin' // role
}
```

### JSDoc Spacing

**❌ Evitare:**
```javascript
// bad - poor JSDoc formatting
/**
*Creates a new user
*@param{string}name-The user's name
*@param{string}email-The user's email
*@returns{Object}The created user
*/
function createUser(name,email) {
  return {name,email}
}
```

**✅ Preferire:**
```javascript
// good - proper JSDoc formatting
/**
 * Creates a new user
 * @param {string} name - The user's name
 * @param {string} email - The user's email
 * @returns {Object} The created user
 */
function createUser(name, email) {
  return { name, email }
}
```

## 🎯 Best Practices Summary

### Formatting Rules

1. **Indentation**: 2 spaces (no tabs)
2. **Line length**: Max 100 characters
3. **Spacing**: Spaces around operators and after commas
4. **Braces**: Always use braces for blocks
5. **Semicolons**: Use semicolons (se richiesto da ESLint config)

### Organization Rules

1. **Imports**: External first, then internal, then assets
2. **Functions**: Helpers before main functions
3. **Constants**: At the top after imports
4. **Exports**: At the bottom of file
5. **Blank lines**: Between logical sections

### Consistency Rules

1. **Single style**: Stick to one formatting style
2. **Prettier**: Use Prettier for automatic formatting
3. **ESLint**: Follow ESLint rules for consistency
4. **Team agreement**: Follow team conventions
5. **Documentation**: Comment formatting standards

Queste regole di layout garantiscono codice JavaScript ben organizzato e leggibile in tutti i progetti PANDEV.
