# Quantum-Cellular-Automata
Extending Conway's Game of Life to quantum domain. <br>

**Background**
A hybrid classical-quantum version of Conway's game of life is explored. Apart from this, the simulation also supports vanilla version (original Conway's Game of Life) and continuous version of it (see details below).<br>
The simulation has implementations in both Qiskit and Cirq. The gameplay of the simulation has been programmed using PyGame. <br>

**Gameplay**
Each cell has a state given by a 3-tuple, which also represents the color of a cell in RGB values. This is in contrast with just the two states i.e., alive and dead in the original version of Conway's Game of Life. Depending on the type of simulation only one or all of the color values of a cell can be changed during the evolution. However, while setting the initial configuration of the grid, a user can only change <code>\textcolor{red}{Red}</code> color of a cell (see below in the **Controls** section). <br>

The rules of the evolution are based on custom defined Hamiltonian, which is based on the moore neighbourhood of a cell, but can easily be extended to other hamiltonians based on different neighbourhoods. <br>
All the code is in a single python notebook.<br>
**Controls**:
- <code>W</code>, <code>A</code>, <code>S</code>, <code>D</code> to move to up, left, right, down.
- <code>BACKSPACE</code> to reset the grid to last configuration.
- <code>SPACE</code> to start the evolution of the grid.
- <code>ESC</code> to quit the simulation.
- Left click on a cell repeatedly to increase the "redness" (Red color value) of the cell, once the maximum value of 1 is reached, prolonged left clicking leads decrease in the "redness". In all versions, whether it is vanilla version, continuous or quantum version one can only change the "redness of the cell. While, in the vanilla version, a red cell is treated as "alive" cell, in the continuous version a cell can take different shades of red and in the quantum version, a cell can take any RGB value.
- To run, create an object of <code>GameOfLife</code> class and pass required arguments to <code>simulation_framework</code> i.e., whether the simulation should be 'Classical' or quantum based (Please choose from 'Qiskit' or 'Cirq') and to <code>game_of_life_version</code> which can be 'vanilla' for the well known version of Game of Life or 'continuous' for the continuous version.
