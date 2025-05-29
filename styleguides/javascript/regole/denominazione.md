# Denominazione JavaScript

Convenzioni di naming per progetti PANDEV seguendo best practices JavaScript moderne.

## 🎯 Naming Conventions Overview

### Variables e Functions: camelCase

**❌ Evitare:**
```javascript
// bad - snake_case (non JavaScript style)
const user_name = 'John'
const api_base_url = 'https://api.example.com'

// bad - PascalCase per variabili
const UserName = 'John'
const ApiClient = createClient()

// bad - abbreviations unclear
const usr = getCurrentUser()
const btn = document.querySelector('button')
const e = new Error('Something went wrong')
```

**✅ Preferire:**
```javascript
// good - clear camelCase
const userName = 'John'
const apiBaseUrl = 'https://api.example.com'
const isUserLoggedIn = checkUserStatus()

// good - descriptive names
const currentUser = getCurrentUser()
const submitButton = document.querySelector('button')
const validationError = new Error('Validation failed')

// good - boolean prefixes
const isLoading = false
const hasPermission = true
const canEdit = user.role === 'admin'
```

### Classes e Constructors: PascalCase

**❌ Evitare:**
```javascript
// bad - camelCase for classes
class userService {
  constructor() {}
}

// bad - snake_case
class user_repository {
  constructor() {}
}

// bad - unclear class names
class Helper {
  constructor() {}
}
```

**✅ Preferire:**
```javascript
// good - clear PascalCase
class UserService {
  constructor(apiClient) {
    this.apiClient = apiClient
  }
}

class PaymentProcessor {
  constructor(config) {
    this.config = config
  }
}

// good - descriptive class names
class EmailNotificationService {
  async sendWelcomeEmail(user) {
    // implementation
  }
}

class DatabaseConnectionManager {
  async connect() {
    // implementation
  }
}
```

### Constants: UPPER_SNAKE_CASE

**❌ Evitare:**
```javascript
// bad - camelCase for constants
const maxRetries = 3
const apiUrl = 'https://api.example.com'

// bad - mixed case
const Max_Retries = 3
const API_url = 'https://api.example.com'
```

**✅ Preferire:**
```javascript
// good - UPPER_SNAKE_CASE for constants
const MAX_RETRIES = 3
const API_BASE_URL = 'https://api.example.com'
const DEFAULT_TIMEOUT = 5000
const HTTP_STATUS_CODES = {
  OK: 200,
  NOT_FOUND: 404,
  INTERNAL_SERVER_ERROR: 500
}

// good - grouped constants
const VALIDATION_RULES = {
  MIN_PASSWORD_LENGTH: 8,
  MAX_USERNAME_LENGTH: 50,
  REQUIRED_FIELDS: ['name', 'email']
}
```

### Files e Directories: kebab-case

**❌ Evitare:**
```javascript
// bad - PascalCase files
UserService.js
PaymentProcessor.js

// bad - camelCase files
userService.js
paymentProcessor.js

// bad - snake_case files
user_service.js
payment_processor.js
```

**✅ Preferire:**
```javascript
// good - kebab-case for files
user-service.js
payment-processor.js
email-validator.js

// good - component files (React/Vue)
user-profile.component.js
navigation-menu.component.vue

// good - directory structure
src/
  components/
    user-profile/
    navigation-menu/
  services/
    payment-service.js
    email-service.js
```

## 🔧 Function Naming Patterns

### Action Functions

**❌ Evitare:**
```javascript
// bad - unclear action
function user() {
  return getCurrentUser()
}

// bad - generic naming
function handle(data) {
  processUserData(data)
}

// bad - missing verb
function validation(input) {
  return input.length > 0
}
```

**✅ Preferire:**
```javascript
// good - clear actions with verbs
function createUser(userData) {
  return new User(userData)
}

function validateEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
}

function processPayment(amount, method) {
  // implementation
}

// good - specific action names
function formatCurrency(amount, locale = 'en-US') {
  return new Intl.NumberFormat(locale, {
    style: 'currency',
    currency: 'USD'
  }).format(amount)
}
```

### Boolean Functions e Variables

**❌ Evitare:**
```javascript
// bad - unclear boolean meaning
const active = user.status === 'active'
const permission = checkUserRole(user)

// bad - negative boolean names
const notLoggedIn = !user.isAuthenticated
const disabled = !button.enabled
```

**✅ Preferire:**
```javascript
// good - clear boolean indicators
const isActive = user.status === 'active'
const isUserActive = checkUserStatus(user)
const hasPermission = checkUserPermission(user, 'write')

// good - positive boolean names
const isLoggedIn = user.isAuthenticated
const isEnabled = button.enabled
const canEdit = user.role === 'admin'

// good - boolean function naming
function isValidEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)
}

function hasRequiredFields(data) {
  return ['name', 'email'].every(field => data[field])
}

function canUserAccessResource(user, resource) {
  return user.permissions.includes(resource.permission)
}
```

### Async Functions

**❌ Evitare:**
```javascript
// bad - no indication of async nature
function getUserData(id) {
  return fetch(`/users/${id}`)
}

// bad - unclear async operation
async function user(id) {
  return await api.getUser(id)
}
```

**✅ Preferire:**
```javascript
// good - clear async operations
async function fetchUserData(id) {
  const response = await fetch(`/users/${id}`)
  return response.json()
}

async function loadUserProfile(userId) {
  const user = await api.getUser(userId)
  const profile = await api.getUserProfile(userId)
  return { ...user, profile }
}

// good - specific async actions
async function saveUserPreferences(userId, preferences) {
  return await api.updateUser(userId, { preferences })
}

async function uploadFile(file) {
  const formData = new FormData()
  formData.append('file', file)
  return await api.upload(formData)
}
```

## 📦 Module e Import Naming

### Import Naming

**❌ Evitare:**
```javascript
// bad - unclear imports
import u from './user-service'
import p from './payment-processor'

// bad - conflicting names
import { User } from './models'
import { User as UserComponent } from './components'

// bad - generic names
import * as utils from './utils'
import * as helpers from './helpers'
```

**✅ Preferire:**
```javascript
// good - descriptive import names
import userService from './user-service'
import paymentProcessor from './payment-processor'

// good - clear aliasing
import { User as UserModel } from './models'
import { User as UserComponent } from './components'

// good - specific namespace imports
import * as dateUtils from './utils/date'
import * as validationHelpers from './helpers/validation'

// good - destructured imports with aliases
import {
  validateEmail as isValidEmail,
  validatePassword as isValidPassword
} from './validators'
```

### Export Naming

**❌ Evitare:**
```javascript
// bad - default export without clear name
export default {
  get: () => {},
  post: () => {},
  delete: () => {}
}

// bad - unclear export names
export const helper = () => {}
export const util = () => {}
```

**✅ Preferire:**
```javascript
// good - named exports
export const apiClient = {
  get: (url) => fetch(url),
  post: (url, data) => fetch(url, { method: 'POST', body: data }),
  delete: (url) => fetch(url, { method: 'DELETE' })
}

// good - clear default export
const userService = {
  async getUser(id) {
    return await apiClient.get(`/users/${id}`)
  },
  
  async createUser(userData) {
    return await apiClient.post('/users', userData)
  }
}

export default userService

// good - multiple related exports
export const userValidators = {
  validateEmail: (email) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email),
  validateAge: (age) => age >= 18 && age <= 120
}

export const userFormatters = {
  formatName: (first, last) => `${first} ${last}`,
  formatEmail: (email) => email.toLowerCase().trim()
}
```

## 🎨 Event Handler Naming

### Event Handlers

**❌ Evitare:**
```javascript
// bad - unclear event handlers
function click() {
  // handle click
}

function change() {
  // handle change
}

// bad - generic naming
function handler(event) {
  // handle event
}
```

**✅ Preferire:**
```javascript
// good - specific event handler names
function handleSubmitClick(event) {
  event.preventDefault()
  submitForm()
}

function handleEmailChange(event) {
  setEmail(event.target.value)
}

function handlePasswordVisibilityToggle() {
  setShowPassword(!showPassword)
}

// good - React/Vue style handlers
const onUserSubmit = (userData) => {
  createUser(userData)
}

const onFormReset = () => {
  resetFormFields()
}

// good - descriptive callback names
function onUserAuthenticationSuccess(user) {
  redirectToProfile(user)
}

function onPaymentProcessingError(error) {
  showErrorMessage(error.message)
}
```

## 🔗 API e Service Naming

### API Methods

**❌ Evitare:**
```javascript
// bad - unclear API methods
const api = {
  user: (id) => fetch(`/users/${id}`),
  users: () => fetch('/users'),
  create: (data) => fetch('/users', { method: 'POST' })
}
```

**✅ Preferire:**
```javascript
// good - RESTful API naming
const userApi = {
  getUser: (id) => fetch(`/users/${id}`),
  getUsers: (params) => fetch(`/users?${new URLSearchParams(params)}`),
  createUser: (userData) => fetch('/users', {
    method: 'POST',
    body: JSON.stringify(userData)
  }),
  updateUser: (id, userData) => fetch(`/users/${id}`, {
    method: 'PUT',
    body: JSON.stringify(userData)
  }),
  deleteUser: (id) => fetch(`/users/${id}`, { method: 'DELETE' })
}

// good - service methods
const emailService = {
  sendWelcomeEmail: (user) => {},
  sendPasswordResetEmail: (email) => {},
  sendNotificationEmail: (user, notification) => {}
}
```

## 📝 Best Practices Summary

### Naming Guidelines

1. **Descriptive**: Nome descrive chiaramente il purpose
2. **Consistent**: Usa convenzioni consistent nel progetto
3. **Concise**: Evita nomi troppo lunghi ma mantieni chiarezza
4. **Searchable**: Evita abbreviazioni che rendono difficile la ricerca
5. **Pronounceable**: Il nome dovrebbe essere pronunciabile

### Conventions Summary

| Type | Convention | Example |
|------|------------|---------|
| Variables/Functions | camelCase | `userName`, `getUserData()` |
| Classes/Constructors | PascalCase | `UserService`, `PaymentProcessor` |
| Constants | UPPER_SNAKE_CASE | `MAX_RETRIES`, `API_BASE_URL` |
| Files/Directories | kebab-case | `user-service.js`, `payment-utils/` |
| Booleans | is/has/can prefix | `isValid`, `hasPermission`, `canEdit` |
| Event Handlers | handle/on prefix | `handleClick`, `onSubmit` |

Seguendo queste convenzioni, il codice JavaScript PANDEV sarà più leggibile, maintainable e consistent across il team.
