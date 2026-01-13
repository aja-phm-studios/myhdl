# Core Components

This directory contains the core system components and fundamental building blocks for the Electroplate-Store-v1 project.

## Purpose

The core directory houses essential components that form the foundation of the system:
- System controllers and state machines
- Basic hardware interfaces
- Common primitives used across modules
- Core data structures and types

## Organization

Components should be organized by functionality:
- One component per file when practical
- Related components can be grouped in subdirectories
- Use clear, descriptive filenames

## Guidelines

### Component Design
- Keep components focused and single-purpose
- Design for reusability
- Document interfaces clearly
- Include usage examples in docstrings

### Testing
- Each component should have corresponding unit tests in `/tests/unit/`
- Test all public interfaces
- Cover edge cases and error conditions

### Example Component Structure

```python
from myhdl import *

@block
def example_core_component(clk, reset, input_signal, output_signal):
    """
    Brief description of the component.
    
    Parameters:
    -----------
    clk : Signal(bool)
        Clock signal
    reset : Signal(bool)
        Reset signal (active high)
    input_signal : Signal(intbv)
        Input data
    output_signal : Signal(intbv)
        Output data
        
    Returns:
    --------
    Instances : list
        MyHDL instances for simulation/synthesis
    """
    
    @always_seq(clk.posedge, reset=reset)
    def logic():
        # Component logic here
        pass
    
    return logic
```

## Components

(To be added as components are developed)

- Component 1: Description
- Component 2: Description
- ...
