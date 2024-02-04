# Maximally Independent Sets and Rydberg Blockades: A Pathway to Quantum Advantage
> In this project, we use Bloqade, QuEra's neutral atom emulator and SDK running on qBraid platform to examine the Maximaly Independent Set (MIS) problem
>
## Description
MIS is the largest subset of graph G such that each vertice next to each other are not connected by an edge. In our particular problem, we determine the paramenters of quantum adiabatic algorithm of neutral atoms to solve the MIS for a defective square grid of vertices with 30% vertice-drop rate.
<p align="center">
  <img width="391" alt="Screenshot 2024-02-04 at 7 31 54 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/da963e0a-d1fa-4ed5-addf-935b6dd240cc">
  <img width="220" alt="Screenshot 2024-02-04 at 7 32 41 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/602c3025-903d-4edf-903f-38292eb1181a">

</p>

## The Problem
Many real-world phenomena can be represented as graph networks, making combinatorial optimization problems involving graphs of paramount importance. Unfortunately, these systems often face combinatorial explosion, where the growth in complexity of computing a solution with additional added nodes makes solutions unfeasible. Quantum algorithms can often exploit novel phenomena that provide a dramatic speedup over classical algorithms. Quantum advantage in combinatorial optimization could prove transformative to many industries and is worth investigating.

Rydberg atom array computers are particularly useful for these problems, as a graph can be mapped onto the atomic array, with each atom serving as a vertex in the graph. The edges are represented by atomic interactions, specifically the Rydberg blockade: when an atom is excited by a laser from its ground state to a highly excited Rydberg state, it blocks its neighbors in a certain radius from transitioning as well. This simulates the unit disk. The lattice spacing translates over directly to atomic spacing. The solution is encoded into the system's Hamiltonian, which evolves with carefully chosen laser pulses. After evolution, assuming everything works as intended, the atoms remaining in the ground state will exactly be those corresponding to the MIS.

## Theory of Implementation
### Solving Math Problems with Lasers
Rydberg atom array Hamiltonian has three main undetermined parameters: Rabi frequency, laser detuning, and phase. The adiabatic apprach define a linear/constant piecewise functions to run on hardware. The detuning $\delta$ is related to Rydberg blockade radius and is fixed by lattice spacing of 4 µm and desired disk radius (Rb/a) ~3

<p align="center">
   <img width="380" alt="Screenshot 2024-02-04 at 7 33 55 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/30994511-d260-4504-9b56-e0cd02480868">

</p>

We first found Rb by taking the geometric mean of $R\_{max}$ and $R\_{min}$
<p align="center">
$R_b = a * \sqrt{3*\sqrt{10}.} = 3.08$
</p>
Lattice constant a is fixed to 4 micrometers due to Aquila physical constraints. C_6 is given as 862690 (*2pi)
