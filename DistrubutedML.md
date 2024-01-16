# Distributed ML

- 数据并行（Data Parallelism）

  在不同的GPU上计算一批数据的不同子集。

  - 各节点分别计算梯度
  - 计算各节点的平均梯度
  - 分别计算每个节点的新参数（相同）

- 模型并行（Model Parallelism）

  - 流水并行（Pipeline Parallelism）

    在不同的GPU上计算模型的不同层，

    - 将一个batch数据分成多个microbatch，每个节点每次处理一个microbatch，原本等待的时间可以用来计算下一个microbatch
    - GPipe方案：每个节点连续进行前向和反向传播
    - PipeDream方案：每个节点交替进行前向和反向传播

  - 张量并行（Tensor Parallelism）

    将单个数学运算（如矩阵乘法）拆分到不同的GPU上，可以将矩阵沿行列拆分成多个shards

- 专家混合（Mixture-of-Experts）

  - 模型中的某些模块包含多组权重
  - 每组数据动态选择其中一组权重进行推理

# Distributed RL

一般将智能体解耦为两部分：Actor 与 Learner。其中 Actor 负责与环境交互获取数据，而 Learner 则利用收集而来的数据进行批量更新。

## A3C

- 在单个机器上使用多个 CPU 线程进行采样与训练，主要用于提高单机性能。相较于 DQN 使用回放缓存来稳定训练过程，A3C 可以通过并行的多个 Actor 来探索环境的不同部分，进而克服了相继状态之间的相关性问题。
- 在 A3C 中，每个 worker 会循环执行以下步骤：

1. 从全局网络同步网络参数
2. 与环境交互采样数据
3. 利用数据计算更新梯度
4. 异步回传梯度给主线程

## IMPALA

Actor 只被设计来采集数据，随后将其传给 Learner，由 Learner 来计算梯度。

采用异步方式使得 Learner 能够持续更新。对于异步带来的异策略问题， IMPALA 进一步提出了 v-trace[1] 算法，以对采样而来的数据进行修正。

## SEED RL

神经网络推理集中由Learner进行，通过确保模型参数和状态不出本地，从而加速推理，并避免数据传输的瓶颈。

## DD-PPO

- 所有Worker既是采样节点，也是训练节点。
- 在每次迭代中，每一个Worker都会经过下面几个步骤： （1）跟环境交互，采样并收集训练样本；（2）收集到足够样本之后，计算模型梯度；（3）所有的Worker进行一次全规约(Allreduce)操作，得到更新后的模型。

![image-20231226231917659](C:\Users\11620\AppData\Roaming\Typora\typora-user-images\image-20231226231917659.png)

## Kaiwu Distributed PPO

