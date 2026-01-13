# Examples

This directory contains example code and demonstrations for the Electroplate-Store-v1 project.

## Purpose

Examples demonstrate:
- How to use core components
- Common design patterns
- Integration scenarios
- Complete working systems
- Best practices

## Organization

Examples should be organized by complexity and topic:
```
examples/
├── basic/              # Simple, introductory examples
├── intermediate/       # More complex examples
├── advanced/          # Advanced usage and optimization
└── complete_systems/  # Full system examples
```

## Guidelines

### Writing Examples

1. **Clear Purpose**: Each example should have a clear learning objective
2. **Self-Contained**: Examples should run independently
3. **Well-Commented**: Explain what the code does and why
4. **Progressive**: Build from simple to complex
5. **Practical**: Show real-world usage

### Example Structure

```python
"""
Example: Basic Counter Implementation

This example demonstrates how to create a simple counter using MyHDL
components from the Electroplate-Store-v1 project.

Learning Objectives:
- Using @block decorator
- Creating sequential logic with @always_seq
- Working with Clock and Reset signals
- Basic simulation

Prerequisites:
- MyHDL installed
- Basic Python knowledge
"""

from myhdl import *

@block
def simple_counter(clk, reset, count, enable, max_count):
    """
    A simple up-counter with enable and reset.
    
    Parameters:
    -----------
    clk : Signal(bool)
        Clock signal
    reset : Signal(bool)
        Active-high reset
    count : Signal(intbv)
        Current count value (output)
    enable : Signal(bool)
        Counter enable (counts when True)
    max_count : int
        Maximum count value before wrapping
    
    Description:
    -----------
    This counter increments on each clock edge when enabled.
    It resets to 0 when reset is asserted, and wraps back to 0
    when reaching max_count.
    """
    
    @always_seq(clk.posedge, reset=reset)
    def counter_logic():
        """Sequential logic for counter."""
        if enable:
            if count >= max_count - 1:
                count.next = 0
            else:
                count.next = count + 1
    
    return counter_logic


def simulate_counter():
    """
    Simulate the counter and display results.
    
    This function demonstrates how to:
    1. Create signals
    2. Instantiate a component
    3. Create test stimulus
    4. Run simulation
    5. Display results
    """
    
    # Create signals
    clk = Signal(bool(0))
    reset = Signal(bool(0))
    count = Signal(intbv(0)[8:])  # 8-bit counter
    enable = Signal(bool(1))
    
    # Instantiate the counter
    counter_inst = simple_counter(
        clk=clk,
        reset=reset,
        count=count,
        enable=enable,
        max_count=10
    )
    
    @instance
    def clock_gen():
        """Generate clock signal."""
        while True:
            yield delay(5)
            clk.next = not clk
    
    @instance
    def stimulus():
        """Test stimulus and monitoring."""
        print("Starting counter simulation...")
        print("-" * 40)
        
        # Apply reset
        reset.next = True
        yield delay(10)
        reset.next = False
        print(f"Reset released, count = {count}")
        
        # Let counter run
        for i in range(25):
            yield clk.posedge
            print(f"Clock cycle {i+1}: count = {count}")
        
        # Disable counter
        print("\nDisabling counter...")
        enable.next = False
        yield clk.posedge
        yield clk.posedge
        yield clk.posedge
        print(f"Counter disabled, count = {count}")
        
        # Re-enable counter
        print("\nRe-enabling counter...")
        enable.next = True
        for i in range(5):
            yield clk.posedge
            print(f"Clock cycle: count = {count}")
        
        print("-" * 40)
        print("Simulation complete!")
        raise StopSimulation
    
    # Run simulation
    sim = Simulation(counter_inst, clock_gen, stimulus)
    sim.run()


def convert_to_verilog():
    """
    Convert the counter to Verilog.
    
    This demonstrates how to convert MyHDL designs to Verilog
    for synthesis or integration with other tools.
    """
    # Create signals
    clk = Signal(bool(0))
    reset = Signal(bool(0))
    count = Signal(intbv(0)[8:])
    enable = Signal(bool(1))
    
    # Convert to Verilog
    counter_inst = simple_counter(
        clk=clk,
        reset=reset,
        count=count,
        enable=enable,
        max_count=256
    )
    
    counter_inst.convert(hdl='Verilog')
    print("Verilog conversion complete! See simple_counter.v")


def convert_to_vhdl():
    """
    Convert the counter to VHDL.
    
    This demonstrates how to convert MyHDL designs to VHDL
    for synthesis or integration with other tools.
    """
    # Create signals
    clk = Signal(bool(0))
    reset = Signal(bool(0))
    count = Signal(intbv(0)[8:])
    enable = Signal(bool(1))
    
    # Convert to VHDL
    counter_inst = simple_counter(
        clk=clk,
        reset=reset,
        count=count,
        enable=enable,
        max_count=256
    )
    
    counter_inst.convert(hdl='VHDL')
    print("VHDL conversion complete! See simple_counter.vhd")


if __name__ == '__main__':
    """
    Main entry point.
    
    Run this script to:
    1. Simulate the counter
    2. Convert to Verilog
    3. Convert to VHDL
    """
    print("=" * 50)
    print("Simple Counter Example")
    print("=" * 50)
    print()
    
    # Run simulation
    simulate_counter()
    print()
    
    # Convert to HDLs
    print("Converting to hardware description languages...")
    convert_to_verilog()
    convert_to_vhdl()
    
    print()
    print("=" * 50)
    print("Example complete!")
    print("=" * 50)
```

## Running Examples

### Basic Execution

```bash
# Run an example
python examples/basic/counter_example.py

# Run with Python's interactive mode
python -i examples/basic/counter_example.py
```

### Using Examples in Your Code

```python
# Import example components
from examples.basic.counter_example import simple_counter

# Use in your own designs
my_counter = simple_counter(clk, reset, count, enable, 100)
```

## Example Categories

### Basic Examples
- Simple counters and registers
- Basic state machines
- Signal manipulation
- Clock and reset handling

### Intermediate Examples
- Multi-module designs
- Communication protocols
- Data processing pipelines
- Control systems

### Advanced Examples
- Performance optimization
- Complex state machines
- System-level integration
- Hardware/software co-design

### Complete Systems
- Full electroplating control system
- Inventory management system
- Transaction processing system
- Safety monitoring system

## Learning Path

**Beginners** should start with:
1. `basic/counter_example.py` - Understanding blocks and signals
2. `basic/state_machine_example.py` - State machine basics
3. `basic/testbench_example.py` - Testing and simulation

**Intermediate Users** can explore:
1. `intermediate/multi_module_example.py` - Component composition
2. `intermediate/protocol_example.py` - Communication protocols
3. `intermediate/pipeline_example.py` - Data processing

**Advanced Users** might study:
1. `advanced/optimization_example.py` - Performance tuning
2. `advanced/complex_system_example.py` - Large-scale designs
3. `complete_systems/` - Full system implementations

## Example Template

Use this template for new examples:

```python
"""
Example: [Title]

Brief description of what this example demonstrates.

Learning Objectives:
- Objective 1
- Objective 2
- Objective 3

Prerequisites:
- Prerequisite 1
- Prerequisite 2

Usage:
    python examples/category/example_name.py
"""

from myhdl import *

# Your example code here

if __name__ == '__main__':
    # Main execution
    pass
```

## Contributing Examples

To contribute a new example:

1. Choose appropriate category (basic/intermediate/advanced)
2. Use the example template above
3. Include clear comments and documentation
4. Add a description to this README
5. Test thoroughly before submitting

## Resources

- [MyHDL Manual](http://docs.myhdl.org/en/stable/manual/)
- [MyHDL Examples](http://www.myhdl.org/examples/)
- Project documentation in `/docs/`
- Core examples in `/example/` (root directory)
