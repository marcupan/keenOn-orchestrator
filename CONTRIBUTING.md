# Contributing to KeenOn Card Orchestration

Thank you for your interest in contributing to the KeenOn Card Orchestration project! This document provides guidelines and instructions for contributing to the project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Testing Guidelines](#testing-guidelines)
- [Documentation Guidelines](#documentation-guidelines)

## Code of Conduct

Please read and follow our [Code of Conduct](CODE_OF_CONDUCT.md) to foster an inclusive and respectful community.

## Getting Started

1. Fork the repository on GitHub
2. Clone your fork locally:
   ```bash
   git clone https://github.com/your-username/orchestrator-repo.git
   cd orchestrator-repo
   ```
3. Set up the development environment:
   ```bash
   # Copy the example environment file
   cp .env.example .env.development
   # Edit .env.development with your configuration
   ```
4. Start the development environment:
   ```bash
   docker-compose -f docker-compose.yml -f docker-compose.development.yml up -d
   ```

## Development Workflow

1. Create a new branch for your feature or bugfix:
   ```bash
   git checkout -b feature/your-feature-name
   # or
   git checkout -b fix/your-bugfix-name
   ```

2. Make your changes, following the [Coding Standards](#coding-standards)

3. Test your changes:
   - Run unit tests for the specific service
   - Run integration tests if applicable
   - Ensure all linting passes

4. Commit your changes following the [Commit Message Guidelines](#commit-message-guidelines)

5. Push your branch to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```

6. Create a Pull Request following the [Pull Request Process](#pull-request-process)

## Pull Request Process

1. Ensure your PR addresses a specific issue. If an issue doesn't exist, create one first.
2. Update the README.md or documentation with details of changes if applicable.
3. The PR should work in all environments (development, production).
4. Include screenshots or examples if your changes include visual elements or new features.
5. Your PR must pass all CI checks before it can be merged.
6. Your PR needs approval from at least one maintainer.

## Coding Standards

### General Guidelines

- Write clean, readable, and maintainable code
- Follow the principle of DRY (Don't Repeat Yourself)
- Keep functions and methods small and focused on a single responsibility
- Use meaningful variable and function names
- Add comments for complex logic, but prefer self-documenting code

### Language-Specific Guidelines

#### TypeScript/JavaScript (Node.js Service)

- Follow the ESLint configuration provided in the project
- Use async/await instead of callbacks or raw promises
- Use TypeScript types and interfaces for better type safety
- Format code using Prettier

```bash
# Run linting
pnpm lint

# Format code
pnpm prettier-format
```

#### Python (Translation Service)

- Follow PEP 8 style guide
- Use type hints where applicable
- Format code using Black

```bash
# Run linting
flake8 src

# Format code
black src
```

#### Rust (Image Composition Service)

- Follow the Rust API guidelines
- Use Rust idioms and patterns
- Format code using rustfmt

```bash
# Run linting
cargo clippy

# Format code
cargo fmt
```

## Commit Message Guidelines

We follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <description>

[optional body]

[optional footer(s)]
```

Types:
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation only changes
- `style`: Changes that do not affect the meaning of the code
- `refactor`: A code change that neither fixes a bug nor adds a feature
- `perf`: A code change that improves performance
- `test`: Adding missing tests or correcting existing tests
- `chore`: Changes to the build process or auxiliary tools

Examples:
```
feat(api): add endpoint for user authentication
fix(translate): resolve issue with character encoding
docs(readme): update setup instructions
```

## Testing Guidelines

- Write unit tests for all new code
- Ensure existing tests pass with your changes
- Write integration tests for new features
- Aim for high test coverage, especially for critical paths

### Running Tests

#### Node.js Service
```bash
cd services/keenOn-card-generate
pnpm test
```

#### Python Service
```bash
cd services/keenOn-card-translate
pytest
```

#### Rust Service
```bash
cd services/keenOn-card-compose
cargo test
```

## Documentation Guidelines

- Update documentation when changing functionality
- Document all public APIs, functions, and methods
- Keep README files up to date
- Use clear and concise language
- Include examples where appropriate

---

Thank you for contributing to the KeenOn Card Orchestration project!
