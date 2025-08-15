# Cursor Rules: Software Design Principles
*Based on "A Philosophy of Software Design" by John Ousterhout*

## Core Directives

**ALWAYS prioritize simplicity over cleverness. When generating code, choose the most straightforward implementation that meets requirements.**

## Module Design Rules

### Rule 1: Create Deep Modules
- Functions should be 20-100 lines with simple interfaces (max 3-4 parameters)
- Classes should have 3-7 public methods but can contain complex internal logic
- AVOID: Thin wrapper functions that just call another function with different parameter names
- PREFER: Modules that encapsulate significant functionality behind simple APIs

### Rule 2: Hide Implementation Details
- Make all internal data structures private/protected
- Never expose implementation classes in public APIs
- Use dependency injection instead of hard-coded dependencies
- PATTERN: Factory methods for object creation instead of exposing constructors with complex parameters

### Rule 3: Design Simple Interfaces
- Method names must clearly indicate their purpose: `getUserById()` not `getUser()`
- Boolean parameters are code smells - use enums or separate methods instead
- AVOID: Methods with more than 4 parameters
- PREFER: Configuration objects for methods requiring many parameters

## Error Handling Rules

### Rule 4: Handle Errors at the Right Level
- Catch exceptions where you can meaningfully handle them, not just to re-throw
- Create custom exception types for domain-specific errors
- NEVER return null - use Optional, Result types, or throw exceptions
- PATTERN: Use a global error handler for unhandled exceptions, not scattered try-catch blocks

### Rule 5: Fail Fast and Clearly
- Validate inputs at method entry points
- Use guard clauses instead of nested if statements
- Provide specific error messages that include context and suggested fixes
- PATTERN: `require(condition, "specific error message")` at method start

## Code Organization Rules

### Rule 6: Eliminate Shallow Modules
- DELETE any class with only getters/setters and no business logic
- MERGE classes that are always used together
- AVOID: Service classes with only one method
- REFACTOR: If a class has <50 lines and no complex logic, inline it

### Rule 7: Keep Related Code Together
- Group related functions in the same file/module
- Co-locate data structures with the code that operates on them
- AVOID: Utils classes with unrelated static methods
- PATTERN: Feature-based folder structure, not layer-based

### Rule 8: Optimize for the Common Case
- Design primary APIs for the 80% use case
- Make common operations require minimal code
- Advanced features can have more complex APIs
- PATTERN: Provide both simple and advanced versions of APIs

## Naming Rules

### Rule 9: Use Intention-Revealing Names
- Variable names should explain their purpose: `userAccountBalance` not `balance`
- Function names should be verbs: `calculateTax()` not `tax()`
- Class names should be nouns: `OrderProcessor` not `ProcessOrder`
- AVOID: Abbreviations, technical jargon, and domain-specific acronyms

### Rule 10: Names Should Match Abstraction Level
- High-level functions use business terms: `processPayment()`
- Low-level functions use technical terms: `encryptData()`
- AVOID: Implementation details in high-level names

## Documentation Rules

### Rule 11: Write Comments for Design Decisions
- Explain WHY you chose an approach, not WHAT the code does
- Document non-obvious trade-offs and assumptions
- REQUIRED: Comments for any code that took >30 minutes to write correctly
- PATTERN: `// Using X instead of Y because of Z performance/maintainability concern`

### Rule 12: Keep Documentation Close to Code
- Put API documentation in the same file as the interface
- Update comments when changing related code
- DELETE outdated comments immediately

## Implementation Guidelines

### Rule 13: Start Simple, Evolve Carefully
- Implement the simplest version that works first
- Add complexity only when requirements demand it
- REFACTOR: When adding the third similar feature, extract the common pattern
- YAGNI: Don't implement features for hypothetical future needs

### Rule 14: Design for Testability
- Write testable code: pure functions, dependency injection, minimal static dependencies
- If a function is hard to test, it's too complex - refactor it
- PATTERN: Constructor injection for dependencies, builder pattern for complex objects

### Rule 15: Measure and Reduce Complexity
- Functions with >3 levels of nesting need refactoring
- Functions with >7 parameters need redesign
- Classes with >10 public methods need splitting
- Cyclomatic complexity >10 requires immediate refactoring

## Code Review Triggers

ALWAYS refactor when you encounter:
- Functions longer than 50 lines
- Classes with both data and unrelated utility methods
- Methods that return null or throw generic exceptions
- Code that requires extensive comments to understand
- Repeated patterns that aren't extracted into reusable components

## Enforcement

- Enable strict linting rules for complexity, function length, and parameter counts
- Use static analysis tools to detect code smells
- Implement automated tests that fail if interfaces become too complex
- Code review checklist must include "Is this the simplest solution?"
