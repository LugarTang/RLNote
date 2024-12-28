Why policy gradient？

Many tasks of interest, most notably physical control tasks, have continuous (real valued) and high dimensional action spaces. E.g. Robotics, car-driving. However, while DQN solves problems with high-dimensional observation spaces, it can only handle discrete and low-dimensional action spaces.

discretize the action space

curse of dimensionality: the number of actions increases exponentially with the number of degrees of freedom. For example, a 7 degree of freedom system (as in the human arm) with the coarsest discretization $a_i \in \lbrace−1, 0, 1\rbrace$ for each joint leads to an action space with dimension 3^7 = 2187.

# Framework

Suppose $\theta$ is the parameter for policy $\pi(\theta)$.

The update rule of $\theta$ is
$$
\theta_{t+1}=\theta_t+\alpha\nabla J\big(\pi(\theta)\big).
$$
The basic derivation refers to [here](https://spinningup.openai.com/en/latest/spinningup/rl_intro3.html).
