# Quantum-Cellular-Automata
Extending [Conway's Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) to quantum domain. <br>

## Background
Cellular automaton (plural, automata) is a simulation of a many body system, in which each body in the *universe* (the collection of bodies in the system) has a  defined *state* and the evolution of the *state* of a body depends on one *fixed and very specific rule*, applied over and over again to get the resultant states of subsequent *generations* or *iterations*. The rule is typically such that only some *neighbourhood* of a body can influence its state and hence, the universe has a concept of *locality*. <br>

Sometimes these automata are termed as 'Zero-player' games since, these objects have rules (like in a game) but once the game begins, it does not require any further external input to keep the *universe* (or maybe in the language of games, the game board or game scene) evolving. <br>

These objects are of considerable interest because, it has been proven that many such systems are Turing-complete, which basically means they are capable of performing computation. This is in turn means, that there exist systems which can have very different paradigm of computing but are equivalent in power to a Turing-machine. Moreover, such systems can also model or rather can be interpreted, as natural (biological) systems such as bacteria colonies in a very simplistic manner. However, mostly the interest in these systems can be attributed to these systems being prototypical examples of chaotic systems and the emergence of aesthetic fractal-like patterns as a result of repeated application of simple (local) rules which can span a possibly infinite universe (see [Gosper's Gun](https://en.wikipedia.org/wiki/Gun_(cellular_automaton)) in Conway's Game of Life). <br>

Conway's Game of Life is one of the well known cellular automata created by British mathematician [John Conway](https://en.wikipedia.org/wiki/John_Horton_Conway) who unfortunately, died due to COVID-19 complications on 11<sup>th</sup> April 2020. In this automata, the *universe* is an infinite grid of cells (squares) with two possible *states*, white (*alive*) and black (*dead*) or vice-versa depending on choice (and implementation). Each cell's state in the next *generation* is given by a simple rule (see below). Note, that in Conway's Game of Life, the *locality* or the cells (called the *neighbours*) which can influence the state of a cell are all contained in the [Moore neighbourhood](https://en.wikipedia.org/wiki/Moore_neighborhood) of a cell, which are precisely the eight cells surrounding any given cell in the grid. <br>

In this project, a hybrid classical-quantum version of Conway's game of life is explored. Apart from this, the simulation also supports vanilla version (original Conway's Game of Life) and continuous version of it (see details below).<br>
The classical-quantum version of the simulation has implementations in both Qiskit and Cirq. The gameplay of the simulation has been programmed using PyGame. All the code is in a single Ipython notebook. <br>

## Gameplay
In this project, the *universe* is same as that of in Conway's Game of Life i.e., an infinite grid of black cells (squares). Each cell has a *state* given by a 3-tuple, which also represents the color of the cell in 'RGB' values. This is in contrast with just the two states i.e., *alive* and *dead* states in the original version of Conway's Game of Life. Depending on the version of simulation, only one or all of the color values of a cell can be changed during the evolution (see below). However, when setting the initial configuration of the grid, a user can **only** change red color value of a cell (see below in the **Controls** section). <br>

There are two broad versions of the simulation, namely the *classical* version and *quantum* version (technically, its classical-quantum hybrid, but we will refer to it as quantum here). The *classical* version has further two implementations, the *vanilla* version (original Conway's Game of Life) and the *continuous* version. The *quantum* version also has two implementations, the *Qiskit* version and *Cirq* version. <br>

### Classical version
- **Vanilla version** : This is the original version of the famous Conway's Game of Life. Each cell can be either *dead* (black) or *alive* (red) i.e, only the Red color value of a cell changes in each new generation and can be either 0 (black) or 255 (red), while the rest of the color values (Blue and Green) are always 0. User can only set any cell either completely red (red color value 255) or leave it as it is (completely black, red color value 0). The evolution rule is as follows:
   -  If a cell is **currently red** and it has **exactly two or exactly three neighbours** colored red then, the cell **stays red**.
   -  If a cell is **currently black** and it has **exactly three neighbours** colored red then, the cell is **colored red**.
   -  In cases other than listed above, the cell is **colored black**.  

- **Continuous version** : Notice, that for the vanilla version above, the rule that dictates the next state of the cell is discrete. This version extends the vanilla version to continuous domain. In this version, cell's *state* is represented by a real number between 0 and 1 (which is then multiplied by 255 to get the Red color value of a cell's color, hence a cell's 'shade' of red represents 'chromatically' the cell's state with completely red as 1 and completely black as 0; Blue and Green color values are identically set to 0). The evolution rule is as follows:
   - If a cell's state is <= 0.5 and the sum of the states of the cells in the moore neighbourhood is <code>S</code>), then new cell state is <code>S - 2 if 2 <= S < 3</code> and <code>4 - S if 3 <= S < 4</code>.
   - If a cell's state is > 0.5 and the sum of the states of the cells in the moore neighbourhood is <code>S</code>), then new cell state is <code>S - 1 if 1 <= S < 2</code> and <code>1 if 2 <= S < 3</code> and <code>4 - S if 3 <= S < 4</code>.
   - In cases other than listed above, the cell is **colored black**. <br>

   Note, that at the extreme values of <code>S</code> (upper bound and lower bound of each interval mentioned in the rules above), the cell will behave like vanilla      version, hence this is a natural extension of the vanilla version to continuous domain. However, other custom modifications are possible.

### Quantum version
When extending to quantum domain, one natural idea can be to allow the state of a cell to be a quantum state and evolution of the cell is governed by unitary operators. While, this is a perfectly reasonable extension and some of the variants have been already mentioned in literature<sup>[[1]](https://arxiv.org/pdf/1902.07835), [[2]](https://arxiv.org/pdf/1010.4666.pdf)</sup> we take a slightly different approach here. <br>

Firstly, each cell is acting as a qubit, but during evolution, the state of a cell is represented by the full 3-tuple (all three RGB values) which are *functions of expected values* of Pauli X, Pauli Z and Pauli Y respectively in the *Qiskit* version and Pauli Y, Pauli Z, Pauli X respectively in the *Cirq* version. Although, note that even in the quantum version of the simulation the user is able to change only the first coordinate (or the Red color value) of a cell's state (color).

Now comes the hybrid classical-quantum part. For the quantum part, let S<sub>P</sub> denote the sum of scaled expected values of Pauli operator P (a different sum for each of X, Y, Z) of all cells in the moore neighbourhood of a given cell. Firstly, we calculate the *expected alive strength* for a given cell, which is again a 3-tuple (or a vector or an array) denoted by (A<sub>X</sub>, A<sub>Z</sub>, A<sub>Y</sub>) in *Qiskit* version and (A<sub>Y</sub>, A<sub>Z</sub>, A<sub>X</sub>) in *Cirq* version. For that, we run the following quantum circuit and observe the expectations of observables X, Y, Z in *specific order* for each cell. Let, S<sub>P</sub> be -> SUM OVER ALL CELLS IN MOORE NEIGHBOURHOOD OF GIVEN CELL {g * (2 * P<sup>th</sup> coordinate of a cell - 1)} where, g is some constant (some values like pi, pi/2, 3.06 gives interesting patterns): <br>
   
   In *Qiskit* version : e<sup>-iS<sub>X</sub></sup> o e<sup>-iS<sub>Z</sub></sup> o e<sup>-iS<sub>Y</sub></sup> applied to |0> state and observe the expectation of X, Z, Y to get the RGB values. 'o' is composition of operators. <br>
   
   In *Cirq* version : Y<sup>S<sub>Y</sub></sup> o Z<sup>S<sub>Y</sub></sup> o X<sup>S<sub>Y</sub></sup> applied to |0> state and observe the expectation of Y, Z, X to get the RGB values. 'o' is composition of operators. <br>

Finally, scaling is performed as <code> A<sub>P</sub> = (1 - A<sub>P</sub>)/2</code> for each P. <br>
   
The classical part is a non-linear function of the *expected alive strength* which ultimately gives a cell's new state. We employ a similar rule like in the continuous version, although other rules are possible. The rule is as follows. Each entry in (A<sub>X</sub>, A<sub>Z</sub>, A<sub>Y</sub>) (for *Qiskit* version, but similarly for *Cirq*) calculated in the above quantum part, is used to get the corresponding entry in the cell' state (or equivalently the color values) i.e,, A<sub>X</sub> -> R, A<sub>Z</sub> -> G, A<sub>X</sub> -> B. To get each entry we employ the following (let, A<sub>P</sub> denote *expected alive strength* for given Pauli from X, Z, Y): <br>
   For each P in X, Z, Y (or equivalently, for each R, G, B color values)
   -  If a cells's P<sup>th</sup> coordinate is <= 0.5 and <code>0.2 <= A<sub>P</sub> < 0.3</code> the P<sup>th</sup> coordinate of cell's new state is 1.
   -  If a cells's P<sup>th</sup> coordinate is > 0.5 and <code>0.2 <= A<sub>P</sub> < 0.4</code> the P<sup>th</sup> coordinate of cell's new state is 1. 
   -  In cases other than listed above, the P<sup>th</sup> coordinate of cell's new state is 0. <br>

**Controls**:
- <code>W</code>, <code>A</code>, <code>S</code>, <code>D</code> to move to up, left, right, down.
- <code>BACKSPACE</code> to reset the grid to last configuration.
- <code>SPACE</code> to start the evolution of the grid.
- <code>ESC</code> to quit the simulation.
- Left click on a cell repeatedly to increase the "redness" (Red color value) of the cell, once the maximum value of 1 is reached, prolonged left clicking leads decrease in the "redness". In all versions, whether it is vanilla version, continuous or quantum version one can only change the "redness" of the cell. While, in the vanilla version, a red cell is treated as "alive" cell, in the continuous version a cell can take different shades of red and in the quantum version, a cell can take any RGB value.
- To run, create an object of <code>GameOfLife</code> class and pass required arguments to <code>simulation_framework</code> i.e., whether the simulation should be 'Classical' or quantum based (Please choose from 'Qiskit' or 'Cirq') and to <code>game_of_life_version</code> which can be 'vanilla' for the well known version of Game of Life or 'continuous' for the continuous version.
