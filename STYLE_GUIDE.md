# BarashStyle - LLM Coding Style Guide

## General Principles

### Code Quality
- Write clean, readable, and maintainable code
- Follow DRY (Don't Repeat Yourself) principle
- Keep functions small and focused on a single responsibility
- Use meaningful variable and function names that clearly describe their purpose
- Prefer explicit over implicit code

### Documentation
- Write clear, concise comments for complex logic
- Document all public APIs, classes, and functions
- Keep comments up-to-date with code changes
- Use docstrings for functions, classes, and modules
- Include usage examples in documentation

### Code Organization
- Structure code logically with clear separation of concerns
- Group related functionality together
- Use consistent file and folder naming conventions
- Keep files at a reasonable length (prefer splitting large files)

## Language-Specific Guidelines

### Python
- Follow PEP 8 style guide
- Use type hints for function parameters and return values
- Prefer f-strings for string formatting
- Use list comprehensions for simple transformations
- Handle exceptions explicitly, avoid bare `except:` clauses
- Use `with` statements for resource management
- Prefer `pathlib` over `os.path` for file operations
- Use dataclasses or pydantic for data structures

### JavaScript/TypeScript
- Use `const` by default, `let` when reassignment is needed, never `var`
- Prefer arrow functions for callbacks and simple functions
- Use async/await over raw promises
- Destructure objects and arrays when accessing multiple properties
- Use template literals for string interpolation
- Follow ESLint recommended rules
- Use TypeScript for type safety in larger projects
- Prefer named exports over default exports

### General Programming Patterns
- Prefer composition over inheritance
- Use dependency injection for better testability
- Implement proper error handling and logging
- Write unit tests for critical business logic
- Follow SOLID principles where applicable

## Naming Conventions

### Variables
- Use descriptive names: `user_count` not `uc`
- Boolean variables should be question-like: `is_active`, `has_permission`
- Use plural names for collections: `users`, `items`

### Functions
- Use verb phrases: `get_user()`, `calculate_total()`
- Keep names concise but descriptive
- Avoid generic names like `process()` or `handle()`

### Classes
- Use noun phrases: `UserManager`, `PaymentProcessor`
- Use PascalCase in most languages
- Interfaces should describe capability: `Serializable`, `Comparable`

### Constants
- Use UPPER_SNAKE_CASE: `MAX_RETRY_COUNT`, `DEFAULT_TIMEOUT`

## Code Structure

### Function Length
- Aim for functions under 50 lines
- If longer, consider breaking into smaller functions
- Each function should do one thing well

### File Organization
- Import statements at the top
- Constants after imports
- Helper functions before main functions
- Main execution logic at the bottom

### Indentation and Spacing
- Use consistent indentation (4 spaces for Python, 2 for JavaScript/TypeScript)
- Add blank lines to separate logical blocks
- Limit line length to 100-120 characters
- Add space around operators: `x = 1 + 2` not `x=1+2`

## Error Handling

### Best Practices
- Catch specific exceptions, not generic ones
- Provide meaningful error messages
- Log errors with context information
- Fail fast when appropriate
- Use custom exception classes for domain-specific errors

### Example Pattern
```python
try:
    result = perform_operation()
except SpecificError as e:
    logger.error(f"Operation failed: {e}", exc_info=True)
    raise CustomError(f"Failed to perform operation: {e}") from e
```

## Testing

### General Guidelines
- Write tests for all business logic
- Use descriptive test names: `test_user_creation_with_valid_email()`
- Follow AAA pattern: Arrange, Act, Assert
- Keep tests independent and isolated
- Mock external dependencies
- Aim for high coverage of critical paths

### Test Structure
```python
def test_feature_name():
    # Arrange
    setup_test_data()
    
    # Act
    result = function_under_test()
    
    # Assert
    assert result == expected_value
```

## Version Control

### Commit Messages
- Use present tense: "Add feature" not "Added feature"
- Be descriptive: explain what and why, not how
- Keep first line under 72 characters
- Add detailed description if needed

### Branching
- Use feature branches: `feature/add-user-authentication`
- Use fix branches: `fix/resolve-login-bug`
- Keep branches short-lived
- Rebase before merging when appropriate

## Security

### Best Practices
- Never commit secrets, API keys, or passwords
- Validate and sanitize all user input
- Use parameterized queries to prevent SQL injection
- Implement proper authentication and authorization
- Keep dependencies up-to-date
- Follow principle of least privilege
- Use environment variables for configuration

## Performance

### Optimization Guidelines
- Profile before optimizing
- Focus on algorithmic improvements first
- Cache expensive operations when appropriate
- Use lazy loading for large datasets
- Implement pagination for large collections
- Be mindful of N+1 query problems

## Code Review

### Reviewer Guidelines
- Be constructive and respectful
- Explain the reasoning behind suggestions
- Distinguish between must-fix and nice-to-have
- Look for logic errors, not just style issues
- Check for security vulnerabilities

### Author Guidelines
- Keep pull requests small and focused
- Respond to all comments
- Don't take feedback personally
- Update code based on valid feedback

## LLM-Specific Instructions

When working with AI coding assistants:
- Provide clear context and requirements
- Specify language, framework, and style preferences
- Request explanations for complex solutions
- Ask for test cases alongside implementation
- Review and validate all generated code
- Iterate on solutions with specific feedback

## References

This style guide should be used as a foundation and adapted based on:
- Project-specific requirements
- Team preferences
- Language-specific best practices
- Industry standards for your domain
