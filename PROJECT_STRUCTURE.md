# MyHDL Project Structure

This document outlines the organization of the MyHDL repository and provides guidance for creating new projects based on this framework.

## Core Repository Structure

### Main Directories

#### `/myhdl/` - Core Library
The main MyHDL Python package containing the core hardware description language implementation.
- **Purpose**: Core HDL functionality, signal definitions, simulation engine
- **Key Components**:
  - Signal and simulation classes
  - Block decorators and HDL constructs
  - Conversion tools (to Verilog/VHDL)
  - Testing infrastructure

#### `/example/` - Example Projects
Reference implementations demonstrating MyHDL usage.
- **Purpose**: Educational examples and design patterns
- **Subdirectories**:
  - `arith_lib/` - Arithmetic library examples
  - `cookbook/` - Recipe-style examples
  - `manual/` - Examples from the manual
  - `rs232/` - RS232 communication example
  - `uart_tx/` - UART transmitter example

#### `/cosimulation/` - Co-simulation Support
Platform-specific co-simulation interfaces with HDL simulators.
- **Purpose**: Integration with commercial and open-source simulators
- **Subdirectories**:
  - `cver/` - Cver simulator support
  - `icarus/` - Icarus Verilog support
  - `modelsim/` - ModelSim support
  - `test/` - Co-simulation tests

#### `/doc/` - Documentation
Comprehensive project documentation.
- **Purpose**: User manuals, API documentation, tutorials
- **Contents**: Sphinx-based documentation source files

#### `/scripts/` - Utility Scripts
Helper scripts for development and benchmarking.
- **Purpose**: Development tools and utilities

### Configuration Files

- `setup.py` - Python package installation configuration
- `requirements.txt` - Python dependencies
- `pytest.ini` - Test runner configuration
- `pylintrc` - Code quality configuration
- `tox.ini` - Testing automation configuration
- `.gitignore` - Version control ignore rules

## Creating New Projects

### Project Template Structure

New projects should follow this organization pattern:

```
project-name/
├── README.md              # Project overview and setup instructions
├── src/                   # Source code
│   ├── core/             # Core functionality
│   ├── modules/          # Feature modules
│   └── utils/            # Utility functions
├── tests/                # Test suite
│   ├── unit/            # Unit tests
│   └── integration/     # Integration tests
├── examples/            # Usage examples
├── docs/                # Project-specific documentation
├── config/              # Configuration files
└── requirements.txt     # Project dependencies
```

### Best Practices

1. **Modularity**: Keep distinct functionality in separate folders
2. **Documentation**: Each major folder should have its own README.md
3. **Testing**: Mirror source structure in test directory
4. **Configuration**: Centralize config files in a dedicated directory
5. **Examples**: Provide working examples for common use cases

## Directory Purpose Summary

| Directory | Purpose | Type |
|-----------|---------|------|
| `/myhdl/` | Core HDL library | Library |
| `/example/` | Reference implementations | Examples |
| `/cosimulation/` | Simulator integration | Integration |
| `/doc/` | Documentation | Documentation |
| `/scripts/` | Development utilities | Tools |
| `/projects/` | Derived projects | Projects |

## Projects Directory

The `/projects/` directory is designed to house derived projects that build upon the MyHDL framework while maintaining their own distinct structure and identity.

### Purpose
- Organize related but independent projects
- Maintain separation of concerns
- Enable reuse of the core MyHDL framework
- Facilitate project-specific customization

### Guidelines for Projects

Each project in `/projects/` should:
1. Have a clear, descriptive name
2. Include comprehensive README.md
3. Maintain independent dependencies when needed
4. Follow the project template structure outlined above
5. Document any modifications to the core framework

## Version Control

- Use `.gitignore` to exclude build artifacts, cache files, and dependencies
- Keep project-specific configuration in respective project directories
- Document any shared utilities or configurations

## For Contributors

When contributing to this repository:
1. Follow the existing directory structure
2. Place new examples in `/example/` with appropriate subdirectory
3. Update relevant documentation
4. Add tests for new functionality
5. Ensure compatibility with existing code

## References

- MyHDL Documentation: http://docs.myhdl.org/
- MyHDL Website: http://www.myhdl.org
- Issue Tracker: GitHub Issues
