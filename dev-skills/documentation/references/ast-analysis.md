# AST Analysis Reference

Technical reference for Python AST-based code analysis used in the documentation generator.

## Core Concepts

### Abstract Syntax Tree (AST)

Python's `ast` module parses source code into a tree structure representing the code's syntax. This allows programmatic extraction of:

- Module-level docstrings
- Class definitions and their docstrings
- Function/method signatures
- Type annotations
- Import statements

## ASTAnalyzer Class

### Methods

| Method | Input | Output |
|--------|-------|--------|
| `analyze_module(file_path)` | Python file path | Module info dict |
| `_extract_classes(tree)` | AST tree | List of class dicts |
| `_extract_functions(tree)` | AST tree | List of function dicts |
| `_extract_imports(tree)` | AST tree | List of import dicts |
| `_extract_args(func_node)` | Function node | List of argument dicts |
| `_extract_returns(func_node)` | Function node | Return type string |

### Module Info Structure

```python
{
    "name": "module_name",
    "path": "/path/to/module.py",
    "docstring": "Module docstring",
    "classes": [
        {
            "name": "ClassName",
            "docstring": "Class docstring",
            "methods": [...],
            "bases": ["BaseClass"]
        }
    ],
    "functions": [
        {
            "name": "function_name",
            "docstring": "Function docstring",
            "args": [{"name": "arg1", "annotation": "str"}],
            "returns": "int"
        }
    ],
    "imports": [
        {"type": "import", "module": "os", "alias": null},
        {"type": "from", "module": "typing", "name": "Dict", "alias": null}
    ]
}
```

## Supported Python Constructs

### Docstrings

- Module docstrings (first string literal in file)
- Class docstrings (first string literal in class body)
- Function/method docstrings

### Type Annotations

```python
# Supported
def func(arg: str) -> int: ...
def func(arg: List[str]) -> Dict[str, int]: ...
def func(arg: Optional[str] = None) -> None: ...

# Partially supported (complex generics)
def func(arg: Callable[[int], str]) -> Awaitable[T]: ...
```

### Class Features

- Single and multiple inheritance
- Instance methods
- Class methods (`@classmethod`)
- Static methods (`@staticmethod`)
- Properties (`@property`)

## Limitations

1. **Runtime evaluation** - Cannot evaluate runtime-generated code
2. **Dynamic typing** - Type hints required for full documentation
3. **External dependencies** - Cannot resolve types from external packages
4. **Complex annotations** - Some advanced typing constructs may not render correctly

## Error Handling

The analyzer returns an error dict for files with syntax errors:

```python
{
    "error": "Syntax error in /path/to/file.py: invalid syntax (line 10)"
}
```
