# Brief 

Published in 2014 ICML. If not specified otherwise, all quote is from this original paper.

# Usage&Advantage

Now we are discussing it under the frame of Policy Gradient Algorithms. So we just focus on what's different to Stochastic Policy Gradient Algorithms.

>The deterministic policy gradient is the expected gradient of the action-value function. This simple form means that the deterministic policy gradient can be estimated much more efficiently than the usual stochastic policy gradient. 

In usual stochastic case, we select action by

$$
\pi_\theta(a|s)=\mathcal P[a|s;\theta].
$$

But now we use a deterministic solution:

$$
a=\mu_\theta(s).
$$

> In the stochastic case, the policy gradient integrates over both state and action spaces, whereas in the deterministic case it only integrates over the state space. As a result, computing the stochastic policy gradient may require more samples, especially if the action space has many dimensions.




This algorithms uses average reward return:

$$
J(\mu_\theta):=\int_{\mathcal S}\rho^\mu r(s,\mu_\theta(s))\text d s,
$$

where $\rho^\mu$ is the discounted state distribution concerning policy $\mu$.







$$
\lim_{\sigma\to0}\nabla_\theta J(\pi_{\mu_\theta,\sigma})=\nabla_\theta J(\mu_\theta).
$$

This equation shows that they bear innate similarity so many algorithms can be migrated here.





# Essence

**Content**

*Conclusion*

$$
\nabla_\theta J(\mu_\theta)=E_{s\sim\rho^\mu}[\nabla_\theta\mu_\theta(s)\nabla_a Q^\mu(s,a)\big|{a=\mu_\theta(s)}].
$$


---
**Proof**

Note that you can not apply derivative rules to uncertain functions, like $Q^{\mu_\theta}(s,\mu_\theta(s))$.




1. Settings: the condition and parameters
2. Steps: Use a flowchart to demonstrate how to do this.
3. Code (Optional)



# DDPG

Our contribution here is to provide modifications to DPG, inspired by the success of DQN, which allow it to use neural network function approximators to learn in large state and action spaces online.

Use replay buffer to facilitate convergence.

target network and soft update



We constructed an exploration policy $\mu'$ by adding noise sampled from a noise process $\mathcal N$ to our actor policy

$$
\mu'(s_t)=\mu(s_t|\theta_t^\mu)+\mathcal N.
$$



the parameter sharing in networks can reduce computing complexity.



Loss:

The actor aims to maximize the expected cumulative reward, so its loss function is designed to encourage actions that lead to higher Q-values.

The critic's role is to estimate the Q-value accurately. The critic loss function is a mean squared error between the predicted Q-value and the target Q-value



$\tau\ll1$, Typically $\tau=0.001$.



batch-normalisation



---

TRPO

Trust region policy optimisation

解决梯度更新步长的选择

先选择trust region再选择方向。

怎么选择trust region的大小?



用原来的策略逼近，损失不大 （切线一致）



正则损失用平均来取代最大散度。



PPO

简化的TRPO

控制了策略差异

PPO Clip: 限制了重要性采样比



PPO penalty: 设置目标散度

