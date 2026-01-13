# Utilities

This directory contains utility functions and helper code for the Electroplate-Store-v1 project.

## Purpose

The utils directory provides:
- Common algorithms and functions
- Data converters and formatters
- Test helpers and fixtures
- Simulation utilities
- Helper functions used across components

## Organization

Utilities should be organized by category:
- Group related utilities together
- Use descriptive filenames
- Keep utilities generic and reusable

## Guidelines

### Utility Design
- Keep functions pure when possible (no side effects)
- Make utilities broadly applicable
- Provide clear documentation
- Include usage examples

### Testing
- Test utility functions thoroughly
- Cover edge cases and error conditions
- Ensure utilities are reliable

### Example Utilities

```python
# conversion_utils.py
from myhdl import intbv

def to_signed(value, width):
    """
    Convert unsigned value to signed representation.
    
    Parameters:
    -----------
    value : int or intbv
        Unsigned value to convert
    width : int
        Bit width
        
    Returns:
    --------
    int : Signed value
    """
    if value >= 2**(width-1):
        return value - 2**width
    return value

def to_unsigned(value, width):
    """
    Convert signed value to unsigned representation.
    
    Parameters:
    -----------
    value : int
        Signed value to convert
    width : int
        Bit width
        
    Returns:
    --------
    int : Unsigned value
    """
    if value < 0:
        return 2**width + value
    return value

# test_utils.py
from myhdl import *

def clock_driver(clk, period=10):
    """
    Generate a clock signal for testing.
    
    Parameters:
    -----------
    clk : Signal(bool)
        Clock signal to drive
    period : int
        Clock period in time units
        
    Returns:
    --------
    generator
        MyHDL generator for clock
    """
    half_period = period // 2
    
    @always(delay(half_period))
    def drive():
        clk.next = not clk
    
    return drive

def reset_sequence(reset, clk, duration=10):
    """
    Generate a reset sequence for testing.
    
    Parameters:
    -----------
    reset : Signal(bool)
        Reset signal to drive
    clk : Signal(bool)
        Clock signal for synchronization
    duration : int
        Reset duration in clock cycles
        
    Returns:
    --------
    generator
        MyHDL generator for reset
    """
    @instance
    def sequence():
        reset.next = True
        yield delay(duration)
        reset.next = False
    
    return sequence
```

## Utility Categories

### Data Conversion
- Type conversions
- Format transformations
- Encoding/decoding

### Testing Helpers
- Clock and reset generators
- Test data generators
- Assertion utilities

### Simulation Utilities
- Waveform generation
- Signal monitoring
- Debug helpers

### Common Algorithms
- CRC calculations
- Checksums
- Data validation

## Usage Example

```python
from src.utils.conversion_utils import to_signed, to_unsigned
from src.utils.test_utils import clock_driver, reset_sequence

# In your test or design
clk = Signal(bool(0))
reset = Signal(bool(0))

# Generate clock
clk_gen = clock_driver(clk, period=10)

# Generate reset sequence
rst_gen = reset_sequence(reset, clk, duration=5)
```
