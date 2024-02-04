# MIT iquHACK QuEra Challenge - Plancktons!!!

[Presentation Slides](https://docs.google.com/presentation/d/10n9njLF_h0AUi2PBZi9EJYwLfpbKsBRMFqhwXGXCmdE/edit?usp=sharing)

### Theory
There were many parameters we found that were possible to tune in the parameter space. Namely, omega, delta and time evolution duration were found to be key factors that needed to be optimized. Going from the logical encoding to the spatial/geometrical encoding has constraints related to the hardware and a neutral atom quantum computer provides a great analogy for this problem. The atoms act as the individual nodes spatially, and the blockade radius Rb relates directly to delta and strong/weak interactions. As such a geometrical encoding, aware of these constraints needs to be made, after which the Hamiltonian is initialized in its ground state and then time evolved following the pulse sequence from rabi (omega) and detuning (tuning). We should adjust delta dynamically to omega for the MIS problem.

A unit disk larger than King's implies modification to the Rb/a value, indicating the radius of the "disk" used to determine the connectivity between vertices. The specific machine, in this case Aquila, utilizes the rydberg_h class from bloqade, which is described as "Create a rydberg program with uniform detuning, amplitude, and phase". We create our variables delta and omega, reperesenting detuning and amplitude respectively, combined with the graph coordinates, it can be run by the Aquila machine for solving IS/MIS problems. The delta value, precisely, is calculated based on the C6/Rb^6 variable, which determines the upper and lower bound. 

<img src="./pictures/vary.png" width="500" height="250">

### Programming
As we started performing simulations, we quickly realised that scaling up the grid by even one more node per row and column would result in straining the capabilities for simulation on our hardware. We know that each qubit doubles the RAM requirement because qubit memory requirement scales up by 2^N. We found that 4x4 lattices wouldn't cross that computational threshold, but some 5x5 lattices did and certainly could not get any 6x6 to work. Varying the complexity from Rb/a = sqrt(2*sqrt(2)) to Rb/a ~ 3 also changes the computational intensity of simulation, as expected. 

We implemented two major protocols to optimize measuring an MIS:
1. The QAOA method: We found that this worked surprisingly well for more Rb/a ~ 3 cases. For example, the MIS was indentified correctly for the following graphs, and we also found that the optimization algorithm always had the MIS in the top 3 solutions.



<img src="./pictures/qaoa_result.png" width="300" height="250">
<img src="./pictures/qaoa_result_2.png" width="300" height="250">



This meant that we just needed to implement some postprocessing (an attempt of which you can find in the QAOA_experimental notebook). However, we had some object type issues when trying to utilize the Bloqade MIS methods. The general idea of the method was to loop through the top 10 options that had highest probabilities, then eliminate those that don't classify as an independent set and then find the maximum suggested independent set in that top 10. omega, delta and the time evolution was tuned to do the QAOA method - where omega was kept largely constant (long pulses) and delta needed to be kept close to 0, this ensured that the phase accumulated over time and played an important factor to the Hamiltonian. 

2. The Adiabatic method: For the adiabatic method, omega is set as the trapezoid shape, to keep area maximised under the curve. The variational optimization is for the delta function. The Nelder-Mead optimization is used to optimize for the shape of delta and also delta_max. It was slightly difficult to find initialized parameters that did not cause the optimization to fail (it would find it hard to minimize in the parameter space), and the proposed probability of getting an MSI seemed scarily low for some graphs...which was definitely intersting - especially since we expected adiabatic to far outperform QAOA given the literature we read.
    


The hardness parameter was calculated using the equation from Ebadi et al, HP = D(|MIS| - 1) / [ |MIS| D(|MIS|)] where |MIS| is the optimal number of nodes in the maximally independent set, D(|MIS|) is the degeneracy of the MIS with |MIS| nodes, and D(|MIS| - 1) is the degeneracy of the MIS with |MIS| - 1 nodes. The GenericTensorNetworks package has a solve function that will find the number of independent sets for a given size of the set, as well as the size of the maximally independent sets. By using the solve feature we were able to plug into the hardness parameter equation for any given graph. A notebook in this repository explores finding hardness for different complexity graphs, and we get some pretty hard ones. 

<img src="./pictures/hardness.png" width="500" height="150">



We ran multiple jobs on the Aquila hardware using the Python package, starting with the 5x5 sample, Rb/a = 1, as shown in the challenge statement (5x5 king's lattice). With Rb/a = 1, we calculated the respective delta and omega as parameters for a piecewise linear function over time. The problem was slowly scaled up from 5x5 to the theoretical maximum of 16x16. Due to the limitation in the number of shots per job, the accuracy of MIS decreased as the dimension increased, requiring post-processing for the data. Due to exponential growth in combinations for IS/MIS as nodes increased, certain nodes exhibited the same amount of counts, which requires post-processing to be used to determine which node, if either, fits inside the MIS.

<img src="./pictures/max.png" width="300" height="200">

### Business
We riffed off the idea that if two training examples are identical, we don’t need to train on both of them. This leads to redundant information. Therefore, we would benefit from having less training examples as long as the ones we remove are repetitive. This would result in only a mild decrease in accuracy but a massive decrease in computational cost. 

We chose the example of 10,000 images of skin lesions and their diagnoses. There are 5 possible diagnoses including melanoma and benign keratosis legion. We chose this dataset because of how easy it is to get embeddings for images, although with time we can get almost any type of embedding. 

<img src="./pictures/lesions.png" width="500" height="200">

With these embeddings, we do Principal Component Analysis (PCA) to map the embedding vectors onto a 2-dimensional space. Points that are similar to each other will be close in the space. Then, we perform a transformation that takes the scatter to a lattice grid format. This is possible with multiple methods with various degrees of information loss. The method we explored was snapping onto the coordinate grid in a predefined way, but this loses data. The other method we could not explore due to resource constraints is a machine learning model that approximates the best grid spacing, angle of coordinates, and displacement from the origin. This is similar to the images from the following thread: (https://stackoverflow.com/questions/62946604/fitting-an-orthogonal-grid-to-noisy-coordinates). 

<img src="./pictures/pca1.png" width="400" height="400">

Once we snap the points onto a lattice grid, we run MIS on the resulting graph using a neutral atom quantum computer. This gives us the maximum amount of clusters of data such that each cluster is sufficiently unique. The points selected from this algorithm are the training data that we will use on our algorithm, rather than the whole dataset. 

Lastly, we train the model classically using a neural net, showing promising results and a clear improvement even after controlling for training size (we did this by showing that selecting some subset of random points is far worse than selecting the specific subset of points from MIS).

The one flaw of this algorithm is that the upfront cost of running the MIS is high, although this can be reduced by only running MIS on a handful of training examples at a time (so that we are running MIS 1000 times on 1000 training examples rather than 1 time on 1000000 examples). Since MIS is NP-hard and likely takes exponential time, it is beneficial to split the run of MIS into multiple iterations which also makes it easier to run on a quantum computer. With improvements in neutral atom quantum computers, we can use a quantum computer instead of a classical computer to run MIS and therefore speed up the ML training process.
