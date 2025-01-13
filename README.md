# D-Wave iQuHackathon 2025 Challenges
D-Wave's quantum annealing computers are different than gate-model systems in function, theory and applicability. Due to their size, D-Wave has been able to build quantum-hybrid tools which are in production and providing customer value today. As such, we are focusing our challenge on solving more "real-world" problems. It would be impractical to expect teams to submit a production-ready application for your final project, but your submissions should reflect needs in business and/or everyday life. Multi-vehicle delivery, production/repair scheduling and resource assignment are all fine places to start.

Unlike gate-model quantum computing, quantum annealing is a method for solving optimization problems. Here are some of the criteria we use to evaluate whether or not a problem is a good fit for our products:

* Is it an optimization problem?
  * Is there something to maximize or minimize?
  * What are the decisions that need to be made (the variables)?
  * What are the constraints?
* Is it nonlinear? Our solvers perform best when working on nonlinear (e.g. quadratic) optimization problems.
  * All solvers but the Nonlinear (NL) can work with at most quadratic polynomials in the objective and/or constraints.
  * The NL hybrid solver can work with higher-degree polynomials as well as [other nonlinear functions](https://docs.ocean.dwavesys.com/en/stable/docs_optimization/reference/symbols.html).
* Will it fit?
  * Advantage™ Quantum Processing Units (QPUs) can take Quadratic Unconstrained Binary Optimization (QUBO) or Ising models as inputs. Depending on problem structure, the maximum number of variables can vary from 170 to 5000+ binary variables.
  * The BQM hybrid solver takes QUBOs or Ising models and can work with up to 1M binary variables.
  * The CQM hybrid solver takes constrained models, treating objectives and constraints separately. It can use binary, integer or continuous variables although it does best with mostly binary and integer decision variables. The maximum problem size is 100k constraints and 500k variables.
  * The Nonlinear (NL) hybrid solver takes in its own Directed Acyclic multi-Graph (DAG) models. The model's DAG can have up to 2M nodes. Variable types can be binary, integer, lists (permutations) or sets (combinations).


## How to get Access
D-Wave is not accessible via qBraid, so you will need to reach out to Ken at the event to get access to our solvers via the Leap™ cloud service. Access allows you to interface with any of our hybrid tools (BQM, CQM and Nonlinear [NL] solvers), Adavantage QPUs and even our newest prototype Advantage2™ QPU. You will need to provide a valid email address.

Once you have signed up for Leap, you will need to download the Ocean™ software development kit (SDK) onto your device and configure it properly. Instructions can be found at [this link](https://docs.ocean.dwavesys.com/en/latest/getting_started.html?_gl=1*e54m6d*_gcl_aw*R0NMLjE3Mjk1MzcwNTguRUFJYUlRb2JDaE1JMDg2RnpwT2dpUU1WbjJGSEFSM3NrQTZjRUFBWUFTQUFFZ0tYclBEX0J3RQ..*_gcl_au*MTcxOTQ3MTcyNS4xNzMyMzAzNDAxLjEzMzI1MzE2MzguMTczNjE4MjkzNS4xNzM2MTg0NjM5*_ga*MTAxNTk2ODI0Ny4xNzI5Mjc2NTQ1*_ga_DXNKH9HE3W*MTczNjI3MzgxMC41Ni4wLjE3MzYyNzM4MTAuNjAuMC4w). Alternatively, you can access a pre-made environment with any of our [examples](https://github.com/orgs/dwave-examples/repositories?type=all) through GitHub Codespaces.


## Easier Challenges
* The NL hybrid solver's open source code provides access to [problem generators](https://github.com/dwavesystems/dwave-optimization/blob/main/dwave/optimization/generators.py) for classic, archetypal optimization problems. A simple challenge would be to take one of these generators, place it within a real-world context, modify it to better fit that context and write code to run the solver and interpret the results. For example, you could modify the knapsack problem with nonlinear terms representing objects which cannot be stored together. Can you think of anything else? 
* If you can think of a good, real-world problem that fits the criteria given in the first section of this README, try solving that problem with any of D-Wave's tools! Be prepared to justify your choice.

## Harder Challenge

### Background
We begin with a simple statement of the "Quadratic Assignment Problem" (QAP). The QAP is a famous archetype of problems which has been called "the hardest of the hard" [1,2]. The classic problem statement is as follows: consider a set of $N$ facilities and $N$ locations to place those facilities at. Each facility may or may not exchange material with other facilities, and each location is some distance from every other location. These quantities are described with symmetric, zero-diagonal matrices:
* $f_{jk}$ is a real number and represents the flow of material between facilities $j$ and $k$
* $d_{mn}\geq 0$ is a real number and represents the distance between locations $m$ and $n$

The goal of the QAP is to arrange facilities at locations in a way which minimizes the total cost of transport for all these flowing goods:

$$C = \sum_{jkmn=0}^{N-1} f_{jk}d_{mn}x_{jm}x_{kn},$$

where $x_{\ell p}=1$ if facility $\ell$ is placed at location $p$ and $0$ otherwise. The problem is constrained by the fact that each location can hold at most one facility and each facility can be placed in at most one location:

$$\sum_{j=0}^{N-1} x_{jk}=1\text{   } \forall k,$$
$$\sum_{k=0}^{N-1} x_{jk}=1\text{   } \forall j.$$

**HINT:** You do not need to use binary variables with D-Wave's new hybrid solver.

### Hard Problem Statement

QAP is easy to solve with D-Wave's quantum-hybrid tools so we are going to modify it. Consider a hospital with $N$ locations, some of which need supplies (ICU, NICU, emergency department...) and some of which are supply closets! Depending on the time of year, the demand for supplies from various closets varies, as does the intake of the departments which are using those supplies. Imagine that once per month, you have the opportunity to shift the closets' and departments' locations. This takes time and effort, though, and it's harder if you move Closet 001 on the first floor all the way up to 305 on the fourth! In essence this is a modified QAP with time dependence.
This same problem structure could also be applied to a supply chain in a larger sense, involving manufacturing facilities or even just machines at a single facility.

Let the location matrix $d_{mn}$ be fixed, but give the flow matrix some variation over some number of time steps.  Additionally, make sure to include a detrimental cost to moving facilities and try to make it scale with distance moved. The goal, much like the standard QAP, is to assign facilities to locations. Build a model which optimizes flow $C$ across all the time steps while simultaneously accounting for costs to move facilities. You should frame this in a realistic setting (not necessarily the hospital example) and justify your choices, e.g. how the flow changes, how costly moving facilities is, etc. Vary problem sizes to get a feel for how the problem scales with D-Wave's solvers. If it is too difficult, you may drop the classification of facilities which "need supplies" and those which are "supply closets;" simply assuming that any facility can be placed at any location. 

## References
1. S. Sahni & T. Gonzalez, "P-Complete Approximation Problems," 1976, [University of Minnesota](https://dl.acm.org/doi/pdf/10.1145/321958.321975)
2. C.W. Commander, "A Survey of the Quadratic Assignment Problem, with Applications," 2003, [University of Florida](http://plaza.ufl.edu/clayton8/article.pdf)
