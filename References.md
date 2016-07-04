> How it works

The engine uses the following techniques:

- Time-corrected position Verlet integrator
- Adaptive grid broad-phase detection
- AABB mid-phase detection
- SAT narrow-phase detection
- Iterative sequential impulse solver and position solver
- Resting collisions using resting constraints (Erin Catto, GDC08)
- Temporal coherence impulse caching and warming
- Collision pairs, contacts and impulses maintained by a pair manager
- Approximate Coulomb friction model using friction constraints
- Constraints solved with the Gauss-Siedel method
- Fixed or variable time step
- A basic sleeping strategy

For more information see the post on [Game physics for beginners](http://brm.io/game-physics-for-beginners/).