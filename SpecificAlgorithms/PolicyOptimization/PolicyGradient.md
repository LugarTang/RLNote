Why policy gradient？

Many tasks of interest, most notably physical control tasks, have continuous (real valued) and high dimensional action spaces. E.g. Robotics, car-driving. However, while DQN solves problems with high-dimensional observation spaces, it can only handle discrete and low-dimensional action spaces. 

discretize the action space 

curse of dimensionality: the number of actions increases exponentially with the number of degrees of freedom. For example, a 7 degree of freedom system (as in the human arm) with the coarsest discretization $a_i \in \{−1, 0, 1\}$ for each joint leads to an action space with dimension 3^7 = 2187. 