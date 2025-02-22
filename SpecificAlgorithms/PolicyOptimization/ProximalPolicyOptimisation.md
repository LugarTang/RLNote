> In its full generality PPO refers to a family of algorithms, where the exact choice of the surrogate objective is left as a flexible design choice to the user. As a consequence, several versions of this algorithm have been proposed in the literature. All of them build on the idea of regularizing the distance between the initial and the updated policy, but they differ in their implementation. [11]

简化的TRPO用不那么严格的方法控制了策略差异.

**PPO-Penalty** approximately solves a KL-constrained update like TRPO, but penalizes the KL-divergence in the objective function instead of making it a hard constraint, and automatically adjusts the penalty coefficient over the course of training so that it’s scaled appropriately.(设置目标散度)

**PPO-Clip** doesn’t have a KL-divergence term in the objective and doesn’t have a constraint at all. Instead relies on specialized clipping in the objective function to remove incentives for the new policy to get far from the old policy.(限制了重要性采样比)

# Properties

| On-policy | action space |  |
| --------- | ------------ | - |
| Yes       | both         |  |

# Improvement

[2] investigated the effect of early stopping (in an epoch if the KL divergence reaches a bar then stop updates) in PPO. They pointed out that this trick can be an alternative to tuning on the number of update iterations per epoch.

[3] proposed to use a proximity term on the divergence (**total variance distrance** is used to measure this) between **visitation distributions** to stabilise training and achieve better result. However, it required that the policy is characterised by **normal distribution**. A form of $\phi-$divergence can be obtained after algebraic manipulations [Equation 12, 3]. This expression can be approximated throught off-policy data incurred by the current policy $\pi$. The function $g$ is approximated by NNs.

## Outer-PPO

[10] generalised PPO by decoupling the policy parameter update and the gradient calculation. Three kinds of generalisation are proposed: non-unity learning rate(original PPO can be viewed as fixing learning rate at $1$), momentum update(linear combination of current gradient and historical ones), biased initialisation (the behavior policy is current policy plus a inductive bias). See [Figure 2, 10] for paradigm demonstration.
