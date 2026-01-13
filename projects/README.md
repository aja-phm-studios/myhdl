# Projects Directory

This directory contains derived projects that build upon the MyHDL framework. Each project maintains its own structure, dependencies, and documentation while leveraging the core MyHDL capabilities.

## Overview

The projects directory is designed to organize related but independent implementations that use MyHDL as their foundation. This approach provides:

- **Separation of Concerns**: Each project has its own isolated structure
- **Framework Reuse**: All projects leverage the core MyHDL library
- **Independent Development**: Projects can evolve independently
- **Clear Organization**: Easy to navigate and understand project relationships

## Directory Structure

Each project should follow this standard structure:

```
project-name/
├── README.md              # Project-specific documentation
├── src/                   # Source code
│   ├── core/             # Core functionality
│   ├── modules/          # Feature modules
│   └── utils/            # Utility functions
├── tests/                # Test suite
│   ├── unit/            # Unit tests
│   └── integration/     # Integration tests
├── examples/            # Usage examples
├── docs/                # Additional documentation
├── config/              # Configuration files
└── requirements.txt     # Project-specific dependencies
```

## Current Projects

### Electroplate-Store-v1
A project designed for electroplate systems management and store operations.
- **Purpose**: Digital system design for electroplating store operations
- **Status**: Template/Initial Structure
- **Location**: `./Electroplate-Store-v1/`

## Creating a New Project

To create a new project in this directory:

1. **Create the project directory**
   ```bash
   mkdir projects/your-project-name
   ```

2. **Set up the basic structure**
   ```bash
   cd projects/your-project-name
   mkdir -p src/core src/modules src/utils
   mkdir -p tests/unit tests/integration
   mkdir -p examples docs config
   ```

3. **Create essential files**
   - `README.md` - Project overview and documentation
   - `requirements.txt` - Python dependencies
   - `.gitignore` - Files to exclude from version control

4. **Document your project**
   - Clearly state the purpose and goals
   - Provide setup and installation instructions
   - Include usage examples
   - Document dependencies and requirements

## Best Practices

### Code Organization
- Keep related functionality together in modules
- Use clear, descriptive names for directories and files
- Maintain a consistent code style across the project

### Documentation
- Every project should have a comprehensive README.md
- Document complex algorithms and design decisions
- Provide examples for common use cases
- Keep documentation up-to-date with code changes

### Testing
- Write unit tests for individual components
- Create integration tests for system-level functionality
- Maintain high test coverage
- Use meaningful test names that describe what is being tested

### Dependencies
- List all dependencies in requirements.txt
- Pin versions for reproducibility when necessary
- Minimize external dependencies when possible
- Document why specific dependencies are needed

### Version Control
- Use .gitignore to exclude generated files and build artifacts
- Commit logical, atomic changes
- Write clear commit messages
- Keep sensitive information out of the repository

## Integration with Core MyHDL

Projects in this directory can:
- Import and use core MyHDL functionality
- Extend MyHDL with custom components
- Create domain-specific abstractions
- Build complete systems using MyHDL primitives

## Support and Resources

- **MyHDL Documentation**: http://docs.myhdl.org/
- **Project Structure Guide**: See `/PROJECT_STRUCTURE.md` in the root directory
- **Core Examples**: See `/example/` directory for reference implementations
- **Community**: https://discourse.myhdl.org

## Contributing

To contribute to any project:
1. Follow the project's specific guidelines (see project README.md)
2. Ensure tests pass before submitting changes
3. Update documentation as needed
4. Follow the existing code style and patterns

## License

Individual projects may have their own licenses. Check each project's README.md for specific license information. The core MyHDL framework is available under the LGPL license.
