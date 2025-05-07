# Parallel Change Pattern for Safe Refactoring

The Parallel Change pattern (also known as Expand and Contract) provides a way to implement breaking changes safely, particularly when modifying data structures or APIs. This pattern is especially valuable in environments where downtime is unacceptable.

### Implementation Steps

1. **Expand phase.** Add the new implementation alongside the old one, with the system writing to both but reading from the old implementation.
2. **Migrate phase.** Gradually migrate existing data or clients to the new implementation.
3. **Contract phase.** Once migration is complete, remove the old implementation.

### Benefits for Branching Strategies

- **Reduces the need for long-lived branches.** Breaking changes can be implemented incrementally on the main branch.
- **Enhances deployment safety.** Changes can be rolled back at any stage if issues are detected.
- **Supports continuous delivery.** The system remains operational throughout the change process.
- **Facilitates large-scale refactoring.** Complex architectural changes can be implemented without disrupting development.

# References and Additional Reading

- [Parallel Change Pattern](https://martinfowler.com/bliki/ParallelChange.html)  
- [Expand and Contract Pattern](https://www.tim-wellhausen.de/papers/ExpandAndContract/ExpandAndContract.html)  