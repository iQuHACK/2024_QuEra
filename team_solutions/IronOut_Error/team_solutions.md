# Maximally Independent Sets and Rydberg Blockades: A Pathway to Quantum Advantage
> In this project, we use Bloqade, QuEra's neutral atom emulator and SDK running on qBraid platform to examine the Maximaly Independent Set (MIS) problem
>
> ## Description
> MIS is the largest subset of graph G such that each vertice next to each other are not connected by an edge. In our particular problem, we determine the paramenters of quantum adiabatic algorithm of neutral atoms to solve the MIS for a defective square grid of vertices with 30% vertice-drop rate.

## The Problem

Many real-world phenomena can be represented as graph networks, making combinatorial optimization problems involving graphs of paramount importance. Unfortunately, these systems often face combinatorial explosion, where the growth in complexity of computing a solution with additional added nodes makes solutions unfeasible. Quantum algorithms can often exploit novel phenomena that provide a dramatic speedup over classical algorithms. Quantum advantage in combinatorial optimization could prove transformative to many industries and is worth investigating.

One such graph problem concerns the "Defective King's Lattice". It is a square grid of vertices with some randomly removed. Each vertex is connected to every other vertex within a certain distance (the disk). The aim is to find the Maximal Independent Set (MIS), the largest subset of the graph such that no vertices in the subset are connected. Finding a MIS has extensive applications, primarily in situations where one seeks to find elements that are seperated physically or in some more abstract sense. Although MIS finding is in general an NP Hard problem, fast classical algorithms have functionally solved it for the Defective King's Lattice with a unit disk size. However, changing the disk size to three units (relative to the spacing between elements in the lattice) makes the problem extremely hard for classical computers, opening an avenue for quantum computers to take an advantage.

Rydberg atom array computers are particularly useful for these problems, as a graph can be mapped onto the atomic array, with each atom serving as a vertex in the graph. The edges are represented by atomic interactions, specifically the Rydberg blockade: when an atom is excited by a laser from its ground state to a highly excited Rydberg state, it blocks its neighbors in a certain radius from transitioning as well. This simulates the unit disk. The lattice spacing translates over directly to atomic spacing. The solution is encoded into the system's Hamiltonian, which evolves with carefully chosen laser pulses. After evolution, assuming everything works as intended, the atoms remaining in the ground state will exactly be those corresponding to the MIS.

## Our Approach and Process
We initially started by reading research papers to understand the problem exactly. These papers mainly included "Hardness of the Maximum Independent Set Problem on Unit-Disk Graphs and Prospects for Quantum Speedups" (https://arxiv.org/pdf/2307.09442.pdf), "Quantum speedup for combinatorial optimization with flat energy landscapes" (https://arxiv.org/pdf/2306.13123.pdf), and "Industry applications of neutral-atom quantum computing solving independent set problems" (https://arxiv.org/pdf/2205.08500.pdf).

## Implementation and Results

## Quantum Hardware

We did most of our testing locally on simulators using small lattices. To ensure our solution scaled to larger systems, we ran tests on QuEra's Rydberg array quantum computer Aquila, using the online platform qBraid. 

## Original Challenge Documentation

See the original prompt for this challenge [here](https://github.com/iQuHACK/2024_QuEra).


