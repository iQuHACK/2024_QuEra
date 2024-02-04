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
$R_b = a * \sqrt{3*\sqrt{10}.} = 12.32$
</p>
Lattice constant a is fixed to 4 micrometers due to Aquila physical constraints, given C6=862690(*2pi). 's' represents represents final Delta (detuning) when Omega is 0.
<p align="center">
   <img width="365" alt="Screenshot 2024-02-04 at 7 34 05 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/95c028cf-619d-43b5-be18-9db3649b1d8d">
<img width="196" alt="Screenshot 2024-02-04 at 7 34 15 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/5ca66ad7-27b0-46d8-818d-7aedb0555750">
</p>

### More Physical Constraints & Pulses
- Upper bound on Rabi Pulses on Aquila Hardware: 2.5 MHz (*2pi)
- Adiabatic computing: Detuning value ramps up to value calculated from equation (0.24668 MHz, which is possible)
- Rabi pulse needed (at least at beginning) for activating the Rydberg states. At end, Detuning needs to be able to overpower Rabi value.
- Piecewise functions is optimized 

### Phase and Geometry
While phase is a parameter in Hamiltonian, we ignore it (for now) due its lack of use in literature. Square lattices (0.3 probability of dropout) used to represent the defective king’s lattice, but other shapes have potential for investigating hardness metric
<p align="center">
   <img width="324" alt="Screenshot 2024-02-04 at 7 34 55 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/3887eb76-3b06-4aaa-82a0-ec732a088a90">
</p>

## Programmemd Implementation
### Protocol Overview
1. Start with naive solution, with reasonable values of Ω and Δ
<p align="center">
   <img width="309" alt="Screenshot 2024-02-04 at 7 35 12 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/022b0d6e-a487-4496-82ee-f0fae8d11229">
</p>
2. Optimize the Rabi frequency waveform:
- Parameters: Strength, Tail Start Time
<p align="center">
<img width="316" alt="Screenshot 2024-02-04 at 7 35 28 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/d03ac352-f133-4fb5-9e00-d59dddd2feda">
</p>
3. Optimize the detuning:
- Detuning parameters Δ at 4 points are defined such that  Δ >> Ω at the end
<p align="center">
<img width="637" alt="Screenshot 2024-02-04 at 8 59 06 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/413fe92c-b82b-4be2-819e-bf834a841567">
</p>

**Optimizations**
Nelder-Mead Optimization method was used  to determine the shape of our Rabi Frequency and Detuning pulses. 
Probability of MIS is used as loss function and calculated by classical methods using tensor network / bloqade subspace
<p align="center">
<img width="354" alt="Screenshot 2024-02-04 at 9 06 59 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/3bbc807d-4c64-4a76-b7d8-a660a81c866a">
<img width="1017" alt="Screenshot 2024-02-04 at 9 09 40 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/13a655fb-0c61-4ba2-83aa-12b26b75614e">

</p>



## Aquila Tasks
- Nonzero probabilities of measuring MIS for Rb/a = 3 on Aquila!
- Post-processed MIS probability
  >Classical: 44.6%
  >Quantum: 27.6%
<p align="center">
<img width="470" alt="Screenshot 2024-02-04 at 9 09 57 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/5cb98fbd-4a3c-44d9-b090-fa8a10ad7d22">
</p>
- MIS-related behavior for 8x8 on Aquila with the classically optimized pulses. Classical solution analysis using subspaces on GenericTensorNetworks is still needed

<p align="center">
<img width="290" alt="Screenshot 2024-02-04 at 9 18 25 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/0fcc32bf-b9be-4b07-af90-8ca44cc38cc9">
</p>

## Analysis
We performed an analysis of the "hardness" of various graphs, as measured by two metrics used in the literature: the hardness metric, which relates to the size of the MIS and the number of ways it can be achieved, and the time taken to achieve a solution. Understandably, the time taken increases with lattice size. 
- 

## Application
### Urban/Suburban/Rural Logistics
<p align="center">
<img width="216" alt="TTS Plot" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/af2a82ed-17a7-4cb8-b98a-4b53e9ebfa7e">
</p>

<p align="center">
<img width="216" alt="Screenshot 2024-02-04 at 9 25 55 AM" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/af2a82ed-17a7-4cb8-b98a-4b53e9ebfa7e">

<img width="327" alt="Screenshot 2024-02-04 at 9 06 29" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/c7330788-a75a-49be-8c47-cc1ec439225e">
</p>

### Details
Goal: Maximize number of towns in accordance with constraints.
Vertices in a grid represent patches of land to build on.
Constraints
Centers must be surrounded with urban areas, then suburban, and then rural area/nature.
Two towns cannot interfere with each other’s infrastructure
<p align="center">
<img width="450" alt="Screenshot 2024-02-04 at 9 07 20" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/77d48fae-e4d8-4f68-b594-2101d9a8dcd8">
</p>


### Generalizability
3 concentric, unique levels that surround vertices
Our levels are urban, suburban, and rural infrastructure in plots of land (vertices)
Another application (while abstract) is a map coloring scenario with the aim of maximizing sets of three concentric given colors.
Green, Red, Yellow 
<p align="center">

<img width="372" alt="Screenshot 2024-02-04 at 9 07 44" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/eb75a187-3636-438e-8e81-30b251a03a1f">
</p>

## Scaling and Optimization
- Advanced/other optimization methods (i.e. Bayesian, smoothing, quantum dueling) 
- Can include searching over a richer space, such as optimizing more time intervals.
- Testing different algorithms other than Adiabatic (e.g. QAOA)
Consider related problems, such as weighted maximal independent sets and other graph problems.
<p align="center">
<img width="598" alt="Screenshot 2024-02-04 at 9 08 05" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/c3dc41ff-0f72-4c40-87a8-cef347ab713e">
<img width="262" alt="Screenshot 2024-02-04 at 9 07 52" src="https://github.com/thomasverrill/2024_QuEra/assets/69056626/1e5dbf32-9491-49b5-8f84-1117878788b2">

</p>
