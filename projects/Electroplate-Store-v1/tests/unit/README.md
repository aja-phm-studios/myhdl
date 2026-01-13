# Unit Tests

This directory contains unit tests for individual components of the Electroplate-Store-v1 project.

## Purpose

Unit tests verify the functionality of individual components in isolation:
- Test single components or functions
- Focus on specific behaviors
- Use mocks/stubs for dependencies
- Run quickly and independently

## Organization

Tests should mirror the source code structure:
```
tests/unit/
├── test_core/           # Tests for src/core/
├── test_modules/        # Tests for src/modules/
└── test_utils/          # Tests for src/utils/
```

## Guidelines

### Writing Unit Tests

1. **One test, one behavior**: Each test should verify one specific behavior
2. **Descriptive names**: Use clear test names that describe what is being tested
3. **AAA Pattern**: Arrange, Act, Assert
4. **Independent**: Tests should not depend on each other
5. **Fast**: Unit tests should run quickly

### Test Structure

```python
import pytest
from myhdl import *
from src.core.example_component import example_component

def test_example_component_basic_functionality():
    """Test that the component performs basic operation correctly."""
    # Arrange: Set up test signals and parameters
    clk = Signal(bool(0))
    reset = Signal(bool(0))
    input_sig = Signal(intbv(0)[8:])
    output_sig = Signal(intbv(0)[8:])
    
    # Create instance
    dut = example_component(clk, reset, input_sig, output_sig)
    
    @instance
    def stimulus():
        # Arrange: Initial conditions
        yield delay(1)
        reset.next = True
        yield delay(2)
        reset.next = False
        
        # Act: Apply test input
        input_sig.next = 42
        yield delay(1)
        clk.next = True
        yield delay(1)
        
        # Assert: Check expected output
        assert output_sig == 42, f"Expected 42, got {output_sig}"
        
        raise StopSimulation
    
    # Run simulation
    sim = Simulation(dut, stimulus)
    sim.run()

def test_example_component_reset_behavior():
    """Test that the component resets correctly."""
    # Arrange
    clk = Signal(bool(0))
    reset = Signal(bool(0))
    input_sig = Signal(intbv(0)[8:])
    output_sig = Signal(intbv(0)[8:])
    
    dut = example_component(clk, reset, input_sig, output_sig)
    
    @instance
    def stimulus():
        # Act: Set some state, then reset
        input_sig.next = 100
        yield delay(1)
        reset.next = True
        yield delay(1)
        
        # Assert: Output should be reset
        assert output_sig == 0, f"Expected 0 after reset, got {output_sig}"
        
        raise StopSimulation
    
    sim = Simulation(dut, stimulus)
    sim.run()

def test_example_component_edge_cases():
    """Test edge cases and boundary conditions."""
    # Test maximum values, minimum values, etc.
    pass

@pytest.mark.parametrize("input_value,expected_output", [
    (0, 0),
    (1, 1),
    (255, 255),
])
def test_example_component_values(input_value, expected_output):
    """Test component with various input values."""
    # Parameterized test for multiple values
    pass
```

### Best Practices

1. **Test Coverage**: Aim for high coverage of source code
2. **Edge Cases**: Test boundary conditions and edge cases
3. **Error Conditions**: Test error handling and invalid inputs
4. **Documentation**: Document complex test setups
5. **Fixtures**: Use pytest fixtures for common test setup

### Running Unit Tests

```bash
# Run all unit tests
pytest tests/unit/

# Run specific test file
pytest tests/unit/test_core/test_component.py

# Run with verbose output
pytest -v tests/unit/

# Run with coverage
pytest --cov=src/core tests/unit/test_core/

# Run specific test
pytest tests/unit/test_core/test_component.py::test_example_component_basic_functionality
```

## Test Utilities

Common test utilities should be placed in `tests/conftest.py` or utility modules:

```python
# conftest.py
import pytest
from myhdl import Signal

@pytest.fixture
def clock_signal():
    """Provide a clock signal for tests."""
    return Signal(bool(0))

@pytest.fixture
def reset_signal():
    """Provide a reset signal for tests."""
    return Signal(bool(0))

@pytest.fixture
def standard_signals():
    """Provide a set of standard test signals."""
    return {
        'clk': Signal(bool(0)),
        'reset': Signal(bool(0)),
        'data': Signal(intbv(0)[8:])
    }
```

## Continuous Integration

Unit tests should run automatically on:
- Every commit
- Pull requests
- Before merges

Configure CI to fail if:
- Any test fails
- Coverage drops below threshold
- Tests take too long

## Resources

- [pytest Documentation](https://docs.pytest.org/)
- [MyHDL Testing Guide](http://docs.myhdl.org/)
- See `/example/` for MyHDL testing examples
