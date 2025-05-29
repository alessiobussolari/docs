# Python Style Guide

Guida di stile Python completa per progetti PANDEV general purpose, con Black, flake8 e mypy per qualità e type safety.

## 🎯 Overview

Python è utilizzato per progetti general purpose in PANDEV. Questa guida copre:

- **General Purpose**: Scripting, web development, data science, automation
- **Tools**: Black, flake8, mypy, isort
- **Versions**: Python 3.9+ e 3.11+
- **Best Practices**: PEP 8, type hints, testing

## 📋 Standard Adottati

### Formatter: Black
**Black** per formattazione automatica del codice:

```toml
# pyproject.toml
[tool.black]
line-length = 100
target-version = ['py39', 'py311']
include = '\.pyi?$'
extend-exclude = '''
/(
  migrations
  | venv
  | \.venv
)/
'''
```

### Linter: flake8
**flake8** per code quality e PEP 8 compliance:

```ini
# .flake8
[flake8]
max-line-length = 100
max-complexity = 10
ignore = 
    E203,  # whitespace before ':'
    E501,  # line too long (handled by black)
    W503,  # line break before binary operator
    F401   # imported but unused (handled by other tools)
exclude = 
    .git,
    __pycache__,
    venv,
    .venv,
    migrations,
    build,
    dist
```

### Type Checker: mypy
**mypy** per type checking statico:

```toml
# pyproject.toml
[tool.mypy]
python_version = "3.9"
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = true
disallow_incomplete_defs = true
check_untyped_defs = true
disallow_untyped_decorators = true
no_implicit_optional = true
warn_redundant_casts = true
warn_unused_ignores = true
warn_no_return = true
warn_unreachable = true
strict_equality = true
```

## 🛠️ Setup Rapido

### 1. Installazione Dependencies

```bash
# Core tools
pip install black flake8 mypy isort

# Pre-commit hooks
pip install pre-commit

# Type stubs (se necessario)
pip install types-requests types-PyYAML types-python-dateutil
```

### 2. File pyproject.toml PANDEV

```toml
[build-system]
requires = ["setuptools>=45", "wheel", "setuptools_scm[toml]>=6.2"]
build-backend = "setuptools.build_meta"

[tool.black]
line-length = 100
target-version = ['py39', 'py311']
include = '\.pyi?$'
extend-exclude = '''
/(
    migrations
    | venv
    | \.venv
    | build
    | dist
)/
'''

[tool.isort]
profile = "black"
line_length = 100
multi_line_output = 3
include_trailing_comma = true
force_grid_wrap = 0
use_parentheses = true
ensure_newline_before_comments = true
known_first_party = ["your_package_name"]
sections = ["FUTURE", "STDLIB", "THIRDPARTY", "FIRSTPARTY", "LOCALFOLDER"]

[tool.mypy]
python_version = "3.9"
warn_return_any = true
warn_unused_configs = true
disallow_untyped_defs = true
disallow_incomplete_defs = true
check_untyped_defs = true
disallow_untyped_decorators = true
no_implicit_optional = true
warn_redundant_casts = true
warn_unused_ignores = true
warn_no_return = true
warn_unreachable = true
strict_equality = true

[[tool.mypy.overrides]]
module = [
    "pandas.*",
    "numpy.*",
    "matplotlib.*",
    "requests.*"
]
ignore_missing_imports = true

[tool.pytest.ini_options]
testpaths = ["tests"]
python_files = ["test_*.py", "*_test.py"]
python_classes = ["Test*"]
python_functions = ["test_*"]
addopts = [
    "--strict-markers",
    "--strict-config",
    "--verbose",
    "--cov=src",
    "--cov-report=term-missing",
    "--cov-report=html",
    "--cov-report=xml"
]

[tool.coverage.run]
source = ["src"]
omit = [
    "*/tests/*",
    "*/venv/*",
    "*/.venv/*",
    "*/migrations/*"
]

[tool.coverage.report]
exclude_lines = [
    "pragma: no cover",
    "def __repr__",
    "if self.debug:",
    "if settings.DEBUG",
    "raise AssertionError",
    "raise NotImplementedError",
    "if 0:",
    "if __name__ == .__main__.:"
]
```

### 3. File .flake8

```ini
[flake8]
max-line-length = 100
max-complexity = 10
select = E,W,F,C
ignore = 
    E203,  # whitespace before ':'
    E501,  # line too long (handled by black)
    W503,  # line break before binary operator
    E711,  # comparison to None should be 'if cond is None:'
    E712   # comparison to True should be 'if cond is True:'
exclude = 
    .git,
    __pycache__,
    venv,
    .venv,
    migrations,
    build,
    dist,
    *.egg-info
per-file-ignores =
    __init__.py:F401
    tests/*:F401,F811
```

### 4. Scripts e Makefile

```makefile
# Makefile
.PHONY: format lint type-check test install dev-install

install:
	pip install -r requirements.txt

dev-install:
	pip install -r requirements-dev.txt
	pre-commit install

format:
	black .
	isort .

lint:
	flake8 .

type-check:
	mypy .

test:
	pytest

all-checks: format lint type-check test

clean:
	find . -type d -name __pycache__ -delete
	find . -type f -name "*.pyc" -delete
	rm -rf .coverage htmlcov/ .pytest_cache/ .mypy_cache/
```

## 🐍 Python Version Support

### Multi-Version Development

```python
# Per supportare Python 3.9+ e 3.11+
from __future__ import annotations

import sys
from typing import Union

if sys.version_info >= (3, 10):
    from typing import TypeAlias
else:
    from typing_extensions import TypeAlias

# Type aliases per compatibilità
JSONValue: TypeAlias = Union[str, int, float, bool, None, dict[str, 'JSONValue'], list['JSONValue']]
```

### Version-Specific Features

```python
# Python 3.9+
def process_data(items: list[dict[str, str]]) -> dict[str, list[str]]:
    """Process data using Python 3.9+ generics."""
    result: dict[str, list[str]] = {}
    for item in items:
        # Implementation
    return result

# Python 3.11+ (match-case)
def handle_response(status_code: int) -> str:
    """Handle HTTP response using Python 3.11+ match-case."""
    match status_code:
        case 200:
            return "Success"
        case 400 | 401 | 403:
            return "Client Error"
        case 500 | 502 | 503:
            return "Server Error"
        case _:
            return "Unknown Status"
```

## 📝 Code Style Guidelines

### 1. Type Hints

```python
from typing import Optional, Union, Any, TypeVar, Generic
from collections.abc import Callable, Iterator

# ✅ Always use type hints
def calculate_total(items: list[float], tax_rate: float = 0.1) -> float:
    """Calculate total with tax."""
    subtotal = sum(items)
    return subtotal * (1 + tax_rate)

# ✅ Use Optional for nullable values
def find_user(user_id: int) -> Optional[dict[str, Any]]:
    """Find user by ID."""
    # Implementation
    return None

# ✅ Generic types
T = TypeVar('T')

class Repository(Generic[T]):
    """Generic repository pattern."""
    
    def save(self, entity: T) -> T:
        """Save entity."""
        # Implementation
        return entity
    
    def find_by_id(self, entity_id: int) -> Optional[T]:
        """Find entity by ID."""
        # Implementation
        return None
```

### 2. Naming Conventions

```python
# ✅ Snake case per funzioni e variabili
def process_user_data(user_info: dict[str, str]) -> bool:
    """Process user data."""
    is_valid = validate_user_info(user_info)
    return is_valid

# ✅ Pascal case per classi
class UserManager:
    """Manage user operations."""
    
    def __init__(self, database_url: str) -> None:
        self.database_url = database_url
        self._connection_pool: Optional[Any] = None
    
    # ✅ Private methods con underscore
    def _establish_connection(self) -> None:
        """Establish database connection."""
        pass

# ✅ Constants in UPPER_CASE
DEFAULT_TIMEOUT = 30
MAX_RETRY_ATTEMPTS = 3
API_BASE_URL = "https://api.example.com"

# ✅ Enums con Pascal case
from enum import Enum

class UserStatus(Enum):
    """User status enumeration."""
    ACTIVE = "active"
    INACTIVE = "inactive"
    PENDING = "pending"
```

### 3. Error Handling

```python
import logging
from typing import Optional

logger = logging.getLogger(__name__)

# ✅ Specific exception handling
def safe_divide(a: float, b: float) -> Optional[float]:
    """Safely divide two numbers."""
    try:
        return a / b
    except ZeroDivisionError:
        logger.warning("Division by zero attempted")
        return None
    except TypeError as e:
        logger.error(f"Type error in division: {e}")
        raise

# ✅ Custom exceptions
class ValidationError(Exception):
    """Raised when validation fails."""
    pass

def validate_email(email: str) -> bool:
    """Validate email format."""
    if "@" not in email:
        raise ValidationError(f"Invalid email format: {email}")
    return True
```

## 🏗️ Project Structure

### Recommended Structure

```
project/
├── src/
│   ├── package_name/
│   │   ├── __init__.py
│   │   ├── main.py
│   │   ├── models/
│   │   │   ├── __init__.py
│   │   │   └── user.py
│   │   ├── services/
│   │   │   ├── __init__.py
│   │   │   └── user_service.py
│   │   └── utils/
│   │       ├── __init__.py
│   │       └── helpers.py
├── tests/
│   ├── __init__.py
│   ├── test_main.py
│   └── test_services/
│       └── test_user_service.py
├── docs/
├── requirements.txt
├── requirements-dev.txt
├── pyproject.toml
├── .flake8
├── .gitignore
├── Makefile
└── README.md
```

### __init__.py Pattern

```python
# src/package_name/__init__.py
"""Package for PANDEV Python project."""

__version__ = "1.0.0"
__author__ = "PANDEV Team"

from .main import main_function
from .models.user import User
from .services.user_service import UserService

__all__ = [
    "main_function",
    "User", 
    "UserService"
]
```

## 🧪 Testing Guidelines

### pytest Configuration

```python
# tests/conftest.py
import pytest
from typing import Generator

@pytest.fixture
def sample_user() -> dict[str, str]:
    """Sample user fixture."""
    return {
        "id": "123",
        "name": "Test User",
        "email": "test@example.com"
    }

@pytest.fixture
def database() -> Generator[dict[str, Any], None, None]:
    """Database fixture with setup/teardown."""
    db = create_test_database()
    yield db
    cleanup_test_database(db)
```

### Test Examples

```python
# tests/test_user_service.py
import pytest
from unittest.mock import Mock, patch

from src.package_name.services.user_service import UserService
from src.package_name.models.user import User

class TestUserService:
    """Test UserService class."""
    
    def test_create_user_success(self, sample_user: dict[str, str]) -> None:
        """Test successful user creation."""
        service = UserService()
        
        result = service.create_user(sample_user)
        
        assert result is not None
        assert result.email == sample_user["email"]
    
    @patch('src.package_name.services.user_service.database')
    def test_create_user_database_error(self, mock_db: Mock) -> None:
        """Test user creation with database error."""
        mock_db.save.side_effect = Exception("Database error")
        service = UserService()
        
        with pytest.raises(Exception):
            service.create_user({"email": "test@example.com"})
    
    @pytest.mark.parametrize("email,expected", [
        ("valid@example.com", True),
        ("invalid-email", False),
        ("", False),
    ])
    def test_validate_email(self, email: str, expected: bool) -> None:
        """Test email validation with different inputs."""
        service = UserService()
        
        result = service.validate_email(email)
        
        assert result == expected
```

## 🚀 Performance Best Practices

### Efficient Code Patterns

```python
from functools import lru_cache
from typing import Iterator
import asyncio

# ✅ Use list comprehensions
def process_numbers(numbers: list[int]) -> list[int]:
    """Process numbers efficiently."""
    return [n * 2 for n in numbers if n > 0]

# ✅ Use generators per large datasets
def process_large_dataset(data: list[dict[str, Any]]) -> Iterator[dict[str, Any]]:
    """Process large dataset with generator."""
    for item in data:
        if item.get('active'):
            yield transform_item(item)

# ✅ Cache expensive operations
@lru_cache(maxsize=128)
def expensive_calculation(value: int) -> float:
    """Expensive calculation with caching."""
    # Simulate expensive operation
    return value ** 2 * 3.14159

# ✅ Async operations
async def fetch_user_data(user_id: int) -> dict[str, Any]:
    """Fetch user data asynchronously."""
    async with aiohttp.ClientSession() as session:
        async with session.get(f"/users/{user_id}") as response:
            return await response.json()

async def fetch_multiple_users(user_ids: list[int]) -> list[dict[str, Any]]:
    """Fetch multiple users concurrently."""
    tasks = [fetch_user_data(user_id) for user_id in user_ids]
    return await asyncio.gather(*tasks)
```

## 🔧 Development Tools Integration

### Pre-commit Hooks

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v4.4.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml
      - id: check-added-large-files
      - id: check-merge-conflict
      
  - repo: https://github.com/psf/black
    rev: 23.3.0
    hooks:
      - id: black
        
  - repo: https://github.com/pycqa/isort
    rev: 5.12.0
    hooks:
      - id: isort
        
  - repo: https://github.com/pycqa/flake8
    rev: 6.0.0
    hooks:
      - id: flake8
        
  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.3.0
    hooks:
      - id: mypy
        additional_dependencies: [types-all]
```

### VS Code Configuration

```json
// .vscode/settings.json
{
  "python.defaultInterpreterPath": "./venv/bin/python",
  "python.formatting.provider": "black",
  "python.linting.enabled": true,
  "python.linting.flake8Enabled": true,
  "python.linting.mypyEnabled": true,
  "python.linting.lintOnSave": true,
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.organizeImports": true
  },
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter",
    "editor.insertSpaces": true,
    "editor.tabSize": 4
  }
}
```

## 📚 Risorse

### Documentazione Ufficiale
- [PEP 8 Style Guide](https://pep8.org/)
- [Black Documentation](https://black.readthedocs.io/)
- [flake8 Documentation](https://flake8.pycqa.org/)
- [mypy Documentation](https://mypy.readthedocs.io/)

### Python Version Guides
- [Python 3.9 What's New](https://docs.python.org/3/whatsnew/3.9.html)
- [Python 3.11 What's New](https://docs.python.org/3/whatsnew/3.11.html)

Per configurazioni dettagliate, consulta le sezioni dedicate:
- [Configurazione Black](configurazione-black.md)
- [Configurazione flake8](configurazione-flake8.md)
- [Configurazione mypy](configurazione-mypy.md)
- [Performance Guidelines](performance.md)
- [Regole Specifiche](regole/README.md)
