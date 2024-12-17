# 9.28

Based on sampling, do not need the info of model. So it's very useful in uncertain condition.

First-visit (一次采样中只考虑对状态的第一次访问) monte-carlo gives unbiased estimation, but every-visit monte-carlo does not.

增量式更新

$$
V_N(s)=V_{N-1}(s)+\frac 1N(G^{(N)(1)}-V_{N-1}(s)),
$$

where $V_N(s)$ is the state value in Nth sampling and $G^{(i)(j)}$ is the return in Nth sampling jth visit to $s$.

优点

1. 无需环境模型，只需与环境交互采样样本数据即可学习策略
2. 评估某个状态的价值函数与其它状态无关，从而可以只评估部分重要的状态
3. 由于不使用贝尔曼方程更新，适用于非马尔可夫场景.

缺点

1. 若采样的回合个数不够多，容易收敛到次优策略
2. 由于需要多个回合的回报，通常只适用有限步长的场景

蒙特卡洛策略评估收敛需要两个很强的假设

1. 需要无限多回合产生的数据
2. 需要试探性出发假设产生数据,也即，对于每一对状态动作对(s,a)都有非零概率被选为回合的起点

放松要求: 在每次策略提升时，采用广义策略迭代的思想，不再要求策略改进前就完成策略评估
例如，在每一回合结束时，就使用观测到的回报进行策略评估，然后在该回合访问到的每一个状态
上进行策略的改进

策略迭代比价值迭代收敛慢

一个策略称为软性的，若满足$\forall (s,a)\implies p(a|s)> 0$.

epsilon-greedy策略是保证充分探索的一个简单软性策略

在动态规划方法中，计算⼀个状态的估值是依据它的后继状态的估值的，我们称之为 bootstrap。

![image-20231005224202244](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231005224202244.png)

TD是无模型的

on-policy: 行动策略(used for sampling)与需要更新的目标策略相同：sarsa

$Q(S_t,A_t)\leftarrow Q(S_t,A_t)+\alpha(R_{t+1}+\gamma Q(S_{t+1},A_{t+1})-Q(S_t,A_t))$. And to n step sarsa

off-policy: q-learning, update algorithm is greedy

![image-20231006171302974](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231006171302974.png)

用于估计目标分布的期望值
直接从目标分布中采样很困难或不可行
可以考虑从另一个容易采样的分布中进行采样

3.6 重要性采样 [1]
定义:
·目标策略$\pi$、行为策略$b$， $\pi\neq b$
问题:
·预测问题，即计算目标策略$\pi$下的状态价值$v_\pi$,或者动作价值$q_\pi$
前提:
·目标策略n和行为策略b都是固定的; 采样是用行为策略b产生的

为了用b产生的片段评估$\pi$的价值，我们要求被产生的每个动作，在b下也要能产生:如果$\pi(a|s) > 0$则有$b(a|s)> 0$. 这被称为覆盖

这意味着在那些b和$\pi$行为不同的状态，b 必须有随机性，而目标策略$\pi$可以是确定性策略

use b to evaluate pi, we need to multiply a ratio of importance of actions, which is the ratio of possibilities of taking this route in these policies.

But this can be very inaccurate， 需要较大采样次数

重要性采样比会增大方差

![image-20231227210204903](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231227210204903.png)

![image-20231006172526364](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231006172526364.png)

要加权的原因是，采样次数不够时，未必可以符合b的分布。

balance of TD and MC: so we consider n-step temporal difference method.

and we add a lambda to weight time exponentially

error decomposition prevents repeatative calculation

![image-20231006173149410](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231006173149410.png)

eligibility traces：就是算一个系数

![image-20231227212428722](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231227212428722.png)

$$
V(s)\leftarrow V(s+\alpha\delta_tE_t(s)),\forall s
$$

# 10.19

![image-20231105115642510](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231105115642510.png)

TD estimation is biased

![image-20231105115939425](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231105115939425.png)

No model:假定V,a也不知道会转移到哪里。

![image-20231227214139562](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231227214139562.png)

Tree backup

![image-20231105124256848](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231105124256848.png)

unifying algorithm: n-step $Q[\sigma]$. Only useful in theory analysis.

Dyna 同时学习model

然后在model中进行模拟

unify model-based and model-free

分布模型
· 给出所有可能性及其出现的概率
样本模型
根据概率，每次给出一种可能性

规划有状态空间规划和策略
空间规划
状态空间规划
从初始状态出发，经过一个个中间状态，最终到达终止状态，期间我们找到一个路径，
并对经过的状态进行估值
策略空间规划
从一个随机的策略出发，不断优化切换到更好的策略上去
前面我们讲的都是状态空间规划
两个关键点
通过计算状态估值提升策略
通过向前探索，再反向传播收益来更新估值

![image-20231105170736468](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231105170736468.png)

dyna-q+ for 探索。

prioritized sweeping: 更新多的先来

when ratification is many, sampling is advantageous.

scale larger, make a table is impossible -> function approximation.

传统的监督学习使用一个固定的训练集，在线的强化学习需要使用不断累积的数据
强化学习的目标函数是会变化的，因为采样的策略一直在变化，即使策略不变，因为采样的缘故目标值也会变化

join state to reduce complexity

考虑1000-状态随机游走问题

状态聚合: 每100个标号聚合成一个状态，得到10个状态，只更新这10个状态的估值。落在一个状态的点的估值相同，状态中每个点的采样都用来修改这个状态的估值。从而

1. 减少了状态。
2. 加速了估值过程。
3. 实现了泛化。

Coarse feature

tiling feature

![image-20231105184016905](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231105184016905.png)

明明一个坐标就能表示的信息要搞这么麻烦，就是为了适应线性函数的低表达力。上个神经网络，直接读坐标就行。

# 11.2

use a function to approximate the value.

mini-batch sgd

transformer: combine former information

![image-20231108103919703](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231108103919703.png)

RL characteristic: target moving

![image-20231108105554360](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231108105554360.png)

![image-20231108105852630](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231108105852630.png)

We can alternate them

![image-20231228111119506](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231228111119506.png)

目的是提高效率

# 11.9

![image-20231113195152365](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231113195152365.png)

![image-20231113195328803](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231113195328803.png)

乐观初值+纯贪心：有什么数学的证明吗

如果局面会变，这个是不好的（它算法本身就没有随机性）

UCB

![image-20231113223631607](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231113223631607.png)

No gradient: delta x to calculate sloop to approximate

![image-20231113230609630](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231113230609630.png)

# 11.16

some method on policy rather than value.

Just output a possibility distribution for choosing actions.

positive return raises possibility of this and reduce others, vice versa.

more similar to human thinking

policy approximation function -> gradient optimisation as well

You can use an approximation as long as

1. Its gradient exists.
2. Exploration (randomness) is ensured.

policy gradient theorem ppt

Make the precise math theorem an approximation based on sampling ppt

蒙特卡洛策略梯度算法 REINFORCE

divide possibility to eliminate the advantage of more possible actions.

write as log for simplification: makes more implicit, meaningless.

add a baseline: so one can know whether it's better or worse than baseline - to make it more accurate

减去的是0有什么用？不变期望，改变方差。希望difference tends to 0.

You can use Monte-Carlo to be the baseline.

Actor-Critic

Change Monte-Carlo in REINFORCE with baseline to TD(n)

Actor是策略网络

Critic是用状态估值评价策略

policy allowing set: big - maximum good, small easy to find maximum in set

Three different target function (maybe linear combination?)

# 12.7

Distributive ML

Mutli-agent RL AI

Environment is dynamic.

In this case, other player's strategies affect yours. So apply RL to one player may result in no convergence.

self-play:

fictitious self-play

CTDE: 中心化训练，去中心化执行

TRPO 不保证性能上升，只是几乎
