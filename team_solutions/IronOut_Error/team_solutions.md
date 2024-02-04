# Maximally Independent Sets and Rydberg Blockades: A Pathway to Quantum Advantage
> In this project, we use Bloqade, QuEra's neutral atom emulator and SDK running on qBraid platform to examine the Maximaly Independent Set (MIS) problem
>
## Description
MIS is the largest subset of graph G such that each vertice next to each other are not connected by an edge. In our particular problem, we determine the paramenters of quantum adiabatic algorithm of neutral atoms to solve the MIS for a defective square grid of vertices with 30% vertice-drop rate.

## The Problem
Many real-world phenomena can be represented as graph networks, making combinatorial optimization problems involving graphs of paramount importance. Unfortunately, these systems often face combinatorial explosion, where the growth in complexity of computing a solution with additional added nodes makes solutions unfeasible. Quantum algorithms can often exploit novel phenomena that provide a dramatic speedup over classical algorithms. Quantum advantage in combinatorial optimization could prove transformative to many industries and is worth investigating.

Rydberg atom array computers are particularly useful for these problems, as a graph can be mapped onto the atomic array, with each atom serving as a vertex in the graph. The edges are represented by atomic interactions, specifically the Rydberg blockade: when an atom is excited by a laser from its ground state to a highly excited Rydberg state, it blocks its neighbors in a certain radius from transitioning as well. This simulates the unit disk. The lattice spacing translates over directly to atomic spacing. The solution is encoded into the system's Hamiltonian, which evolves with carefully chosen laser pulses. After evolution, assuming everything works as intended, the atoms remaining in the ground state will exactly be those corresponding to the MIS.
