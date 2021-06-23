# Quantum-Cellular-Automata
Extending [Conway's Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) to quantum domain. <br>

## Background
Cellular automata is a simulation of many body systems, in which each body in the *universe* (the collection of bodies in the system) has a *state* defined and the evolution of the *state* of a body depends on one *fixed and very specific rule*, applied over and over again to get the resultant states of subsequent *generations* or *iterations*. Note, that when the rule is applied on a body, only some *neighbourhood* of the body is considered and hence, the universe has a concept of *locality*. <br>

Sometimes these automata are termed as 'Zero-player' games since, these objects have rules (like in a game) but once the game begins, it does not require any further external input to keep the *universe* (or maybe in the language of games, the game board or game scene) evolving. <br>

These objects are of considerable interest because, it has been proven that many such systems are Turing-complete which basically means they are capable of performing computation. This is in turn means, that there exist systems which can have very different paradigm of computing but are equivalent in power to a Turing-machine. Moreover, such systems can also model or rather can be interpreted, as natural (biological) systems such as bacteria colonies in a very simplistic manner. However, mostly the interest in these systems can be attributed to these systems being prototypical examples of chaotic systems and the emergence of aesthetic fractal-like patterns as a result of repeated application of simple (local) rules which can span a possibly infinite universe (see [Gosper's Gun](https://en.wikipedia.org/wiki/Gun_(cellular_automaton)) in Conway's Game of Life). <br>

Conway's Game of Life is one of the well known cellular automata created by [John Conway](https://en.wikipedia.org/wiki/John_Horton_Conway) who unfortunately, died due to COVID-19 complications on 11<sup>th</sup> April 2020. In this automata, the *universe* is an infinite grid of cells (squares) with two possible *states*, white (*alive*) and black (*dead*) or vice-versa depending on choice. Each cell's state in the next *generation* is given by a simple rule (see below). Note, that in Conway's Game of Life, the *locality* or the cells (called the *neighbours*) which can influence the state of a cell are all contained in the [Moore neighbourhood](https://en.wikipedia.org/wiki/Moore_neighborhood) of a cell, which are precisely the eight cells surrounding any given cell in the grid. <br>

In this project, a hybrid classical-quantum version of Conway's game of life is explored. Apart from this, the simulation also supports vanilla version (original Conway's Game of Life) and continuous version of it (see details below).<br>
The classical-quantum version of the simulation has implementations in both Qiskit and Cirq. The gameplay of the simulation has been programmed using PyGame. <br>

## Gameplay
In this project, the *universe* is same as that of in Conway's Game of Life i.e., an infinite grid of black cells (squares). Each cell has a *state* given by a 3-tuple, which also represents the color of the cell in 'RGB' values. This is in contrast with just the two states i.e., *alive* and *dead* states in the original version of Conway's Game of Life. Depending on the version of simulation, only one or all of the color values of a cell can be changed during the evolution. However, when setting the initial configuration of the grid, a user can **only** change red color value of a cell (see below in the **Controls** section). <br>

There are two broad versions of the simulation, namely the *classical* version and *quantum* version (technically, its classical-quantum hybrid, but we will refer to it as quantum here). The *classical* version has further two implementations, the *vanilla* version (original Conway's Game of Life) and the *continuous* version. The *quantum* version also has two implementations, the *Qiskit* version and *Cirq* version. <br>

### Classical version
- **Vanilla version** : This is the original version of the famous Conway's Game of Life. Each cell can be either *dead* (black) or *alive* (red). User can only set any cell either completely red or leave it as it is (completely black). The evolution rule is as follows:
   -  If a cell is **currently red** and it has **exactly two or exactly three neighbours** colored red then, the cell **stays red**.
   -  If a cell is **currently black** and it has **exactly three neighbours** colored red then, the cell is **colored red**.
   -  In cases other than listed above, the cell is **colored black**.   


The rules of the evolution are based on custom defined Hamiltonian, which is based on the moore neighbourhood of a cell, but can easily be extended to other hamiltonians based on different neighbourhoods. <br>
All the code is in a single python notebook.<br>
**Controls**:
- <code>W</code>, <code>A</code>, <code>S</code>, <code>D</code> to move to up, left, right, down.
- <code>BACKSPACE</code> to reset the grid to last configuration.
- <code>SPACE</code> to start the evolution of the grid.
- <code>ESC</code> to quit the simulation.
- Left click on a cell repeatedly to increase the "redness" (Red color value) of the cell, once the maximum value of 1 is reached, prolonged left clicking leads decrease in the "redness". In all versions, whether it is vanilla version, continuous or quantum version one can only change the "redness" of the cell. While, in the vanilla version, a red cell is treated as "alive" cell, in the continuous version a cell can take different shades of red and in the quantum version, a cell can take any RGB value.
- To run, create an object of <code>GameOfLife</code> class and pass required arguments to <code>simulation_framework</code> i.e., whether the simulation should be 'Classical' or quantum based (Please choose from 'Qiskit' or 'Cirq') and to <code>game_of_life_version</code> which can be 'vanilla' for the well known version of Game of Life or 'continuous' for the continuous version.
