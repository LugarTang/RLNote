# Tricks

[1] investigated some implementation details in **on-policy** RL algorithms on **continuous control** (majorly robotics) problems.

## Action Branching

[8] first proposed this trick to mitigate the network complexity with high-dimensional discrete action space. Like you need to output a tuple $\in\{-1,0,1\}\times\{-1,1\}\times\{0,1,2,3,\cdots,N\}$.

[8] proposed to use a common representation part at first and let separate network to determine the action on each dimension. See [Figure 2, 8].

## Ensemble Action

Train multiple agents and then make a balanced decision, the simplest form is linear combination.

## Policy Parameterization

[7] pointed out that using Beta distribution for continuous action space can not only explicitly incorporate action space boundaries and eliminate the biased boundary effects caused by truncated Gaussian, but also lead to more reliable convergence in some case.

## Frame Skipping

Frame skipping is used to reduce long training time. Yet ignoring observations between observed frames may cause losing detailed control for the environment.

## Control Policy Difference - Clipping

[1] asserted that PPO policy loss with clipping threshold $\epsilon=0.25\in[0.1,0.5]$ is the most generally well-performing policy loss. [1] also stated that gradient clipping might slightly help but is of secondary importance.

[4] proposed an adaptive clipping approach for PPO to address that sampled states have different importance. They aim to make the policy to exhibit less changes at less important state.

> However the clipped probability ratio used by PPO in its surrogate learning objective may allow less important states to receive more policy updates than desirable. This is because policy update at more important states often vanish early during repeated policy optimisation whenever the corresponding probability ratios shoot beyond a given clipping threshold. [4]

[6] proposed functional clipping methods to further penalise policy divergence as the flat clipping of original PPO is still possible to make an update too far away.

See [Figure1, 6] for details.

[7] observed that the clipping mechanism effectively prevents the policy from moving further away **once it is outside the trust region**, but it does not bound the size of an individual policy update step. This behaviour is particularly problematic if a single reward signal can cause the policy to end up in regions with low reward signal. [7] discussed that in environment outside of common continuous control benchmarks, using KL-regularisation may be better than clipping.

## Networks

[1] found that "for the value function there seems to be no downside in using wider networks". Two hidden layers appear to work well for policy and value networks in all tested environments. As for activation functions, we observe that tanh activation performs best and relu worst.

[1] mentioned **policy initialisation scheme** greatly influences performance and they discovered that initial policy that has **zero mean, a rather low standard deviation and is independent of the observation significantly improves the training speed.** (Allow better exploration, I guess.)

This can be achieved by initialising the policy MLP with smaller weights in the last layer (this alone boosts the performance on Humanoid by 66%) so that the initial action distribution is almost independent of the observation and by introducing an offset in the standard deviation of actions.

In summary, [1] recommended that initialise the last policy layer with 100× smaller weights. Use softplus to transform network output into action standard deviation and add a (negative) offset to its input to decrease the initial standard deviation of actions. Tune this offset if possible. Use tanh both as the activation function (if the networks are not too deep) and to transform the samples from the normal distribution to the bounded action space. Use a wide value MLP (no layers shared with the policy) but tune the policy width (it might need to be narrower than the value MLP).

## Normalisation

[1] recommended that always use observation normalisation and check if value function normalization improves performance. A contradiction is raised between [1] and [5]. [1], under the scenario that PPO-style value-clipping is disabled, found that value normalisation hurts the performance on a specific task. Whilst on the same task, [5] discovered that value normalisation improves the performance, yet with PPO-style clipping enabled.

[5] discovered that a reward-scaling trick used in the implementation of openAI PPO accounts for performance gain, where the rewards are divided through by the standard deviation of a rolling discounted sum of the rewards (without subtracting and re-adding the mean). Details refer to the paper.

## Optimizers

[1] recommended using Adam optimiser with momentum $\beta_1 = 0.9$ and a tuned learning rate (0.0003 is a safe default). Linearly decaying the learning rate may slightly improve performance but is of secondary importance.
