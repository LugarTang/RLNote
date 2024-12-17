简化的TRPO

控制了策略差异

**PPO-Penalty** approximately solves a KL-constrained update like TRPO, but penalizes the KL-divergence in the objective function instead of making it a hard constraint, and automatically adjusts the penalty coefficient over the course of training so that it’s scaled appropriately.(设置目标散度)

**PPO-Clip** doesn’t have a KL-divergence term in the objective and doesn’t have a constraint at all. Instead relies on specialized clipping in the objective function to remove incentives for the new policy to get far from the old policy.(限制了重要性采样比)

# Properties

| On-policy | action space |  |
| --------- | ------------ | - |
| Yes       | both         |  |

# Reference

1. [Proximal Policy Optimization — Spinning Up documentation (openai.com)](https://spinningup.openai.com/en/latest/algorithms/ppo.html)
