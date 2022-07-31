## Testing Frameworks

### Packages used

As far as possible, WVSolutions adopts a test-driven development practice. Test development is primarily performed using 
1. xUnit (.NET test framework)
2. bUnit (Blazor unit test framework)
3. Moq is also used for dummy data generation (also known as Mocking).

### Approaches adopted

WVSolution's core functionality relies heavily on the persisted Fluxor states, which significantly reduce the number of database operations performed during the use. As such, the contents of these states are simulated, allowing for the application to still be able to retrieve and load data from states.

### Test coverage

We aim to comprehensively test, as isolated components, the following aspects of the program.

1. Fluxor states and their manipulation
2. Component functionality, separated mostly by use cases
3. Drug calculations
4. System load capacity tests, including stress tests