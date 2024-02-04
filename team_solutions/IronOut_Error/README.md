# IronOut Error - iQuHACK 2024 Challenge Submission

This is IronOut Error's official submission for iQuHACK 2024, with a project from QuEra focused on quantum acceleration of combinatorial optimization.

## The Problem

Many real-world phenomena can be represented as graph networks, making combinatorial optimization problems involving graphs of paramount importance. Unfortunately, these systems often face combinatorial explosion, where the growth in complexity of computing a solution with additional added nodes makes solutions unfeasible. Quantum algorithms can often exploit novel phenomena that provide a dramatic speedup over classical algorithms. Quantum advantage in combinatorial optimization could prove transformative to many industries and is worth investigating.

One such graph problem concerns the "Defective King's Lattice". It is a square grid of vertices with some randomly removed. Each vertex is connected to every other vertex within a certain distance (the disk). The aim is to find the Maximal Independent Set (MIS), the largest subset of the graph such that no vertices in the subset are connected. Finding a MIS has extensive applications, primarily in situations where one seeks to find elements that are seperated physically or in some more abstract sense. Although MIS finding is in general an NP Hard problem, fast classical algorithms have functionally solved it for the Defective King's Lattice with a unit disk size. However, changing the disk size to three units (relative to the spacing between elements in the lattice) makes the problem extremely hard for classical computers, opening an avenue for quantum computers to take an advantage.

Rydberg atom array computers are particularly useful for these problems, as a graph can be mapped onto the atomic array, with each atom serving as a vertex in the graph. The edges are represented by atomic interactions, specifically the Rydberg blockade: when an atom is excited by a laser from its ground state to a highly excited Rydberg state, it blocks its neighbors in a certain radius from transitioning as well. This simulates the unit disk. The lattice spacing translates over directly to atomic spacing. The solution is encoded into the system's Hamiltonian, which evolves with carefully chosen laser pulses. After evolution, assuming everything works as intended, the atoms remaining in the ground state will exactly be those corresponding to the MIS.

## Our Approach and Process

## Implementation and Results

## Quantum Hardware

We did most of our testing locally on simulators using small lattices. To ensure our solution scaled to larger systems, we ran tests on QuEra's Rydberg array quantum computer Aquila, using the online platform qBraid. 

## Original Challenge Documentation

See the original prompt for thsi challenge [here](https://github.com/iQuHACK/2024_QuEra).

## Documentation

This year’s iQuHACK challenges require a write-up/documentation portion that is heavily considered during
judging. The write-up is a chance for you to be creative in describing your approach and describing
your process. It should clearly explain the problem, the approach you used, your implementation with results
from simulation and hardware, and how you accessed the quantum hardware (total number of shots used, 
backends used, etc.).

Make sure to clearly link the documentation into the `README.md` of your own solutions folder and to include a link to the original challenge repository from the documentation!


## Submission

To submit the challenge, do the following:
1. Place all the code you wrote in one folder with your team name under the `team_solutions/` folder (for example `team_solutions/quantum_team`).
2. Create a new entry in `team_solutions.md` following the format shown that links to the folder with your solution and your documentation.
3. Create a Pull Request from your repository to the original challenge repository
4. Submit the "challenge submission" form

Project submission forms will automatically close on Sunday at 10am EST and won't accept late submissions.
