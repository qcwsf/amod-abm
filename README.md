# amod-abm

![Alt Text](https://github.com/wenjian0202/amod-abm/blob/master/media/demo.gif)

## what's amod-abm?

Basically, `amod-abm` is an **a**gent-**b**ased **m**odeling platform for simulating **a**utonomous **m**obility-**o**n-**d**emand systems. It is written in Python 3 and has an [Open Source Routing Machine](https://github.com/Project-OSRM/osrm-backend#open-source-routing-machine) working backend. The simulation platform dipicts *agents* (travelers, vehicles etc.) at the individual level while tackling the traffic in a macroscopic manner (which means, no interaction with other vehicles and no congestion concerns). The current demo is based on a London case study, for which it provides tools to support AMoD system design (fleet sizing, sharing policies, hailing rules, pricing etc.) and experiment with dispatching algorithms including trip-vehicle assignment and real-time rebalancing. 

Almost effortlessly, this toolbox could be used to simulate AMoD systems in any urban setting other than London (as long as we understand the demand there). You're also welcome to extend `amod-abm` according to your own needs. 

Thanks for contributing! 

## what is and what will be included?

As of today, the following parts have been implemented:
- class `Model` for free-floating AMoD systems, with a fleet of vehicles and a central dispatcher which
  - assigns requests to vehicles based on Insertion Heuristics [[1]](http://www.sciencedirect.com/science/article/pii/0191261586900202)
  - (optional) reoptimizes the assignment based on Simulated Annealing [[2]](https://www.researchgate.net/publication/281445468_Dynamic_Shared-Taxi_Dispatch_Algorithm_with_Hybrid_Simulated_Annealing)
  - (optional) rebalances vehicles using either Simple Anticipatpry Rebalancing, Optimal Rebalancing Problem or Deep Q Network [[3]](https://mobility.mit.edu/publications/9999/wen-rebalancing-shared-mobility-demand-systems-reinforcement-learning-approach)
- a predefined demand matrix in `demand.py` with time-invariant demand volume for any OD pair
- class `Veh` for autonomous vehicles
  - vehicle capacity can be set to 1 (no sharing), 2 (at most 2 travelers sharing at a time) or more
- class `Req` for requests
  - requests are generated based on their demand volumes, following Poisson process
  - requests can be either on-demand or in-advance
- class `OSRMEngine` for connecting to the OSRM routing server
  - OSRM should be compiled and map data preprocessed beforehand
  - OSRM is offline (in order to speed up) so only returns static routing
- class `RebalancingEnv` for training the deep Q network
  - it extends [keras-rl](http://keras-rl.readthedocs.io/en/latest/) and works with [Keras](https://keras.io/) and [TensorFlow](https://www.tensorflow.org/)
  - pre-computed DQN weights are in folder `weights` for use
  
The main function in `main.py` will simulate the system given input parameters from `Constants.py`. An example of the results of a typical simulation run could be found in folder `output`. System performance indicators for analysis include wait time, travel time, detour and service rate at the traveler side, as well as vehicle miles traveled, rebalancing distancc and average load at the operator side.

The following features are expected to be available by the end of this year (2017):
- time-variant demand across a day
- automated interaction with the demand prediction models (so as to link with pricing)
- statistics regarding operational cost and revenue

## how do I get started?

