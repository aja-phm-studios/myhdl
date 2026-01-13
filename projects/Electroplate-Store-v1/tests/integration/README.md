# Integration Tests

This directory contains integration tests for the Electroplate-Store-v1 project.

## Purpose

Integration tests verify that multiple components work correctly together:
- Test component interactions
- Verify system-level behavior
- Test complete workflows
- Validate end-to-end scenarios

## Organization

Integration tests should be organized by feature or subsystem:
```
tests/integration/
├── test_process_control/      # Process control system tests
├── test_inventory_system/     # Inventory management tests
├── test_safety_systems/       # Safety monitoring tests
└── test_complete_workflow/    # End-to-end workflow tests
```

## Guidelines

### Writing Integration Tests

1. **Test Real Interactions**: Use actual components, not mocks
2. **Meaningful Scenarios**: Test realistic use cases
3. **Complete Workflows**: Test from input to output
4. **System State**: Verify system state changes correctly
5. **Timing**: Include realistic timing and sequencing

### Test Structure

```python
import pytest
from myhdl import *
from src.core.controller import controller_component
from src.modules.inventory import inventory_module
from src.modules.process_control import process_control_module

def test_inventory_and_process_control_integration():
    """
    Test that inventory and process control modules work together.
    
    Scenario: When a process starts, inventory should be decremented,
    and when process completes, inventory should be updated.
    """
    # Arrange: Set up all components
    clk = Signal(bool(0))
    reset = Signal(bool(0))
    
    # Inventory signals
    item_id = Signal(intbv(0)[8:])
    quantity = Signal(intbv(100)[16:])
    
    # Process control signals
    process_start = Signal(bool(0))
    process_complete = Signal(bool(0))
    
    # Instantiate modules
    inventory = inventory_module(clk, reset, item_id, quantity)
    process_ctrl = process_control_module(
        clk, reset, process_start, process_complete, item_id
    )
    
    @instance
    def clock_gen():
        """Generate clock signal."""
        while True:
            yield delay(5)
            clk.next = not clk
    
    @instance
    def stimulus():
        """Test stimulus."""
        # Reset system
        reset.next = True
        yield delay(10)
        reset.next = False
        yield delay(10)
        
        # Initial inventory check
        initial_quantity = int(quantity)
        print(f"Initial quantity: {initial_quantity}")
        
        # Start process
        item_id.next = 1
        process_start.next = True
        yield clk.posedge
        process_start.next = False
        
        # Wait for process to consume inventory
        yield delay(50)
        
        # Verify inventory was decremented
        current_quantity = int(quantity)
        assert current_quantity < initial_quantity, \
            f"Inventory should decrease: was {initial_quantity}, now {current_quantity}"
        
        # Complete process
        process_complete.next = True
        yield clk.posedge
        process_complete.next = False
        
        yield delay(50)
        
        # Final checks
        print(f"Final quantity: {int(quantity)}")
        
        raise StopSimulation
    
    # Run simulation
    sim = Simulation(inventory, process_ctrl, clock_gen, stimulus)
    sim.run()

def test_complete_transaction_workflow():
    """
    Test complete transaction workflow from start to finish.
    
    Scenario: 
    1. Check inventory availability
    2. Start electroplating process
    3. Monitor process status
    4. Complete transaction
    5. Update records
    """
    # Arrange: Set up complete system
    clk = Signal(bool(0))
    reset = Signal(bool(0))
    
    # System signals
    transaction_id = Signal(intbv(0)[16:])
    status = Signal(intbv(0)[8:])
    
    # Component instances
    # (Add actual components as they are developed)
    
    @instance
    def clock_gen():
        while True:
            yield delay(5)
            clk.next = not clk
    
    @instance
    def stimulus():
        # Reset
        reset.next = True
        yield delay(10)
        reset.next = False
        
        # Test complete workflow
        # 1. Initialize transaction
        transaction_id.next = 12345
        yield clk.posedge
        
        # 2. Verify initial status
        assert status == 0, "Initial status should be 0"
        
        # 3. Execute workflow steps
        # (Add specific workflow steps)
        
        # 4. Verify final state
        yield delay(100)
        
        raise StopSimulation
    
    sim = Simulation(clock_gen, stimulus)
    sim.run()

def test_safety_system_integration():
    """
    Test that safety systems integrate correctly with other modules.
    
    Scenario: Safety monitor should halt processes when limits are exceeded.
    """
    # Test safety integration
    pass

@pytest.mark.slow
def test_long_running_operation():
    """
    Test long-running operations and state persistence.
    
    This test may take longer to run and is marked as slow.
    """
    pass
```

### Best Practices

1. **Realistic Scenarios**: Test actual use cases that will occur in production
2. **Clear Setup**: Make test setup and preconditions explicit
3. **Adequate Timing**: Allow sufficient time for async operations
4. **State Verification**: Check intermediate states, not just final output
5. **Cleanup**: Ensure tests clean up after themselves

### Running Integration Tests

```bash
# Run all integration tests
pytest tests/integration/

# Run specific integration test
pytest tests/integration/test_inventory_system/

# Run with verbose output
pytest -v tests/integration/

# Skip slow tests
pytest -m "not slow" tests/integration/

# Run only slow tests
pytest -m "slow" tests/integration/

# Run with detailed logging
pytest -v -s tests/integration/
```

## Test Data

Integration tests may need realistic test data:

```python
# test_data.py
SAMPLE_INVENTORY = {
    'item_1': {'id': 1, 'quantity': 100, 'name': 'Copper Solution'},
    'item_2': {'id': 2, 'quantity': 50, 'name': 'Nickel Solution'},
    # ...
}

SAMPLE_TRANSACTIONS = [
    {'id': 1001, 'item_id': 1, 'quantity': 5, 'timestamp': '2024-01-01'},
    {'id': 1002, 'item_id': 2, 'quantity': 3, 'timestamp': '2024-01-02'},
    # ...
]
```

## Performance Testing

Integration tests can also verify performance:

```python
import time

def test_system_performance():
    """Verify system meets performance requirements."""
    start_time = time.time()
    
    # Run system operations
    # ...
    
    elapsed = time.time() - start_time
    assert elapsed < 1.0, f"Operation took {elapsed}s, expected < 1.0s"
```

## Test Markers

Use pytest markers to categorize tests:

```python
@pytest.mark.integration
def test_something():
    pass

@pytest.mark.slow
def test_long_operation():
    pass

@pytest.mark.requires_hardware
def test_hardware_integration():
    pass
```

Configure in `pytest.ini`:
```ini
[pytest]
markers =
    integration: Integration tests
    slow: Slow-running tests
    requires_hardware: Tests requiring hardware
```

## Resources

- [pytest Documentation](https://docs.pytest.org/)
- [MyHDL Documentation](http://docs.myhdl.org/)
- See `/tests/unit/` for simpler test examples
