# Configuration

This directory contains configuration files for the Electroplate-Store-v1 project.

## Purpose

Configuration files for:
- System parameters
- Build settings
- Test configurations
- Environment settings
- Deployment configurations

## Organization

```
config/
├── development/        # Development environment configs
├── testing/           # Test environment configs
├── production/        # Production configurations
└── templates/         # Configuration templates
```

## Configuration Types

### System Parameters
Hardware and system configuration parameters:
- Clock frequencies
- Data widths
- Memory sizes
- Timing constraints

### Build Settings
Settings for build and synthesis:
- Target device
- Optimization levels
- Synthesis options
- Place and route settings

### Test Configurations
Test environment settings:
- Test parameters
- Simulation settings
- Coverage requirements
- Test data locations

### Environment Settings
Development and runtime environment:
- Path configurations
- Tool locations
- Library paths
- Debug settings

## Configuration Files

### Example: System Parameters

```python
# system_params.py
"""
System-wide configuration parameters for Electroplate-Store-v1.

These parameters define the system characteristics and can be
adjusted for different implementations or requirements.
"""

# Clock Configuration
CLOCK_FREQUENCY_HZ = 50_000_000  # 50 MHz
CLOCK_PERIOD_NS = 20  # 20 ns

# Data Width Configuration
DATA_WIDTH = 8  # bits
ADDRESS_WIDTH = 16  # bits
COUNTER_WIDTH = 32  # bits

# Memory Configuration
MEMORY_SIZE = 1024  # entries
FIFO_DEPTH = 16  # entries

# Process Configuration
MAX_PROCESSES = 8
PROCESS_TIMEOUT_CYCLES = 10000

# Inventory Configuration
MAX_ITEMS = 256
ITEM_ID_WIDTH = 8

# Safety Parameters
TEMP_THRESHOLD_HIGH = 80  # degrees
TEMP_THRESHOLD_LOW = 20  # degrees
CURRENT_THRESHOLD_MAX = 100  # amperes

# Communication Parameters
BAUD_RATE = 115200
UART_DATA_BITS = 8
UART_STOP_BITS = 1

# Debug Configuration
DEBUG_ENABLED = True
VERBOSE_LOGGING = False
```

### Example: Test Configuration

```python
# test_config.py
"""
Test configuration parameters.
"""

# Test Execution
TEST_TIMEOUT_SEC = 30
MAX_SIMULATION_TIME = 10000  # time units
RANDOM_SEED = 42

# Coverage Requirements
MIN_CODE_COVERAGE = 80  # percent
MIN_BRANCH_COVERAGE = 70  # percent

# Test Data
TEST_DATA_DIR = "./tests/data/"
GOLDEN_REFERENCE_DIR = "./tests/golden/"

# Simulation Settings
WAVEFORM_DUMP_ENABLED = True
WAVEFORM_FORMAT = "vcd"  # or "fst"
```

### Example: Build Configuration

```json
// build_config.json
{
  "target": {
    "device": "xc7a35t",
    "package": "cpg236",
    "speed": "-1"
  },
  "synthesis": {
    "optimization": "speed",
    "resource_sharing": true,
    "retiming": true
  },
  "constraints": {
    "clock_period_ns": 20,
    "input_delay_ns": 2,
    "output_delay_ns": 2
  },
  "output": {
    "format": "edif",
    "directory": "./build/output/"
  }
}
```

## Using Configurations

### In Python Code

```python
# Import configuration
from config.system_params import *

# Use configuration values
@block
def my_component(clk, data):
    """Component using system configuration."""
    counter = Signal(intbv(0)[COUNTER_WIDTH:])
    
    @always_seq(clk.posedge, reset=None)
    def logic():
        if counter >= PROCESS_TIMEOUT_CYCLES:
            # Handle timeout
            pass
    
    return logic
```

### In Test Code

```python
# Import test configuration
from config.test_config import *

def test_with_config():
    """Test using configuration."""
    # Use test configuration
    random.seed(RANDOM_SEED)
    
    # Run simulation with configured timeout
    timeout = TEST_TIMEOUT_SEC
```

### Environment Variables

```bash
# Set environment variables
export ELECTROPLATE_CONFIG_DIR=/path/to/config
export ELECTROPLATE_ENV=development

# Use in scripts
python run_tests.py --config $ELECTROPLATE_CONFIG_DIR/test_config.py
```

## Configuration Management

### Best Practices

1. **Separate by Environment**: Keep dev, test, and prod configs separate
2. **Document Values**: Explain what each parameter does
3. **Use Constants**: Define magic numbers as named constants
4. **Version Control**: Track configuration changes
5. **Validate**: Check configuration values at startup

### Configuration Validation

```python
# config_validator.py
"""Validate configuration parameters."""

def validate_system_params():
    """Validate system configuration parameters."""
    from config.system_params import *
    
    # Check ranges
    assert CLOCK_FREQUENCY_HZ > 0, "Clock frequency must be positive"
    assert DATA_WIDTH in [8, 16, 32], "Unsupported data width"
    assert MEMORY_SIZE > 0, "Memory size must be positive"
    
    # Check relationships
    assert ITEM_ID_WIDTH >= math.log2(MAX_ITEMS), \
        "Item ID width insufficient for max items"
    
    print("Configuration validation passed!")

if __name__ == '__main__':
    validate_system_params()
```

### Configuration Templates

Create templates for new configurations:

```python
# templates/new_project_config.py
"""
Configuration template for new projects.

Copy this file and customize for your project.
"""

# Project Information
PROJECT_NAME = "My Project"
PROJECT_VERSION = "0.1.0"

# System Parameters
# TODO: Customize these values
CLOCK_FREQUENCY_HZ = 50_000_000
DATA_WIDTH = 8

# Add your configuration parameters here
# ...
```

## Environment-Specific Configs

### Development

```python
# development/config.py
"""Development environment configuration."""

DEBUG_ENABLED = True
VERBOSE_LOGGING = True
WAVEFORM_DUMP = True
ASSERTIONS_ENABLED = True
```

### Testing

```python
# testing/config.py
"""Testing environment configuration."""

DEBUG_ENABLED = True
VERBOSE_LOGGING = False
WAVEFORM_DUMP = False
ASSERTIONS_ENABLED = True
COVERAGE_ENABLED = True
```

### Production

```python
# production/config.py
"""Production environment configuration."""

DEBUG_ENABLED = False
VERBOSE_LOGGING = False
WAVEFORM_DUMP = False
ASSERTIONS_ENABLED = False
OPTIMIZATIONS_ENABLED = True
```

## Loading Configuration

```python
# config_loader.py
"""Load configuration based on environment."""

import os
import importlib

def load_config():
    """
    Load configuration based on ELECTROPLATE_ENV environment variable.
    
    Returns:
        module: Configuration module
    """
    env = os.getenv('ELECTROPLATE_ENV', 'development')
    config_module = f'config.{env}.config'
    
    try:
        config = importlib.import_module(config_module)
        print(f"Loaded configuration for {env} environment")
        return config
    except ImportError:
        print(f"Warning: Could not load {env} config, using defaults")
        return None

# Usage
config = load_config()
```

## Security Considerations

### Sensitive Data
- **Never commit** secrets, passwords, or keys
- Use environment variables for sensitive data
- Use secret management tools for production
- Document required secrets without exposing values

### Example: Secrets Management

```python
# secrets.py (add to .gitignore!)
"""
Secrets and sensitive configuration.

DO NOT COMMIT THIS FILE TO VERSION CONTROL!
"""

API_KEY = "your-api-key-here"
DATABASE_PASSWORD = "your-password-here"
```

```python
# secrets_template.py (commit this)
"""
Template for secrets configuration.

Copy to secrets.py and fill in actual values.
"""

API_KEY = "your-api-key-here"
DATABASE_PASSWORD = "your-password-here"
```

## Resources

- See `/PROJECT_STRUCTURE.md` for project organization
- See `/projects/README.md` for project guidelines
- Python ConfigParser: https://docs.python.org/3/library/configparser.html
- JSON: https://docs.python.org/3/library/json.html
