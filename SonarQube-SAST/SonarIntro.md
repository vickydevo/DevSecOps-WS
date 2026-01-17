## What is SonarQube?

SonarQube is an open-source platform for continuous inspection of code quality. It detects bugs, vulnerabilities, and code smells, ensuring maintainability and reliability. It supports over 25 programming languages, including Java, Python, JavaScript, C#, and more.

## Why is it Mainly Used for Java?

- Java's popularity in enterprise applications with complex codebases.
- Robust support for Java-specific rules and plugins.
- Seamless integration with Java build tools like Maven and Gradle.

## Default Port

The default port for accessing SonarQube is **9000**.

## Code Quality Checks in SonarQube

SonarQube performs the following checks:

1. **Bugs**: Logical errors or flaws in the code.
2. **Vulnerabilities**: Security issues exploitable by attackers.
3. **Code Smells**: Maintainability issues or bad coding practices.
4. **Code Coverage**: Measures the percentage of code executed during testing.
5. **Duplications**: Detects duplicate code blocks.
6. **Complexity Analysis**: Identifies overly complex code needing refactoring.

These checks ensure reliable, secure, and maintainable code, integrating seamlessly into CI/CD pipelines.

## What is Code Coverage?

Code coverage measures the percentage of source code executed during testing, identifying untested parts of the codebase.

### Types of Code Coverage

1. **Statement Coverage**: Ensures each statement is executed.
2. **Branch Coverage**: Tests all possible branches (e.g., if-else).
3. **Function Coverage**: Confirms every function is called.
4. **Line Coverage**: Measures executed lines of code.

### Importance

- Identifies testing gaps.
- Improves reliability and maintainability.
- Reduces undetected bugs in production.

### Tools

- **JaCoCo** (Java)
- **Coverage.py** (Python)
- **Istanbul** (JavaScript)
- **Cobertura** (Java)
- **gcov** (C/C++)
- **SonarQube** (multi-language support)

## SonarQube Versions

1. **Community Edition** (Free): Basic features for small teams.
2. **Developer Edition** (Paid): Advanced features for developers.
3. **Enterprise Edition** (Paid): Scalable for large organizations.
4. **Data Center Edition** (Paid): High availability for enterprises.

For details, refer to the [SonarQube Editions Comparison](https://www.sonarsource.com/plans-and-pricing/).
