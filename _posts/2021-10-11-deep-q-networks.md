---
title: 'Reinforcement Learning: Deep Q-networks'
date: 2021-10-11
permalink: /posts/2021/10/rl_dqn/
tags:
  - reinforcement-learning
  - dqn
  - deep-reinforcement-learning
  - 
---

Online Q-learning can experience instabilities during training. This is because by using experience sampled sequentially from the environment leads to highly correlated gradient steps. Deep Q-networks (DQN) made deep reinforcement learning a viable approach to complex sequential control problems.  In this section, we introduce the vanilla DQN algorithm. Next sections will discuss various improvements that have been proposed in the literature.


Deep Q-networks
======

Deep q-networks were introduced in the seminal work of Mnih et al. in [2]. They demonstrated that a single DQN can achive human level performance in many Atari games without any feature engineerign. A DQN modifies online Q-learing in two ways [1, 2]

- Introducing experience replay
- Introducing target network

Both these two features greatly stabilize the learning.  In particular, a DQN stores the experience tuples $(s_i, \alpha_i, r_i, s_{i, NEW})$ in an experience buffer. During training, the  samples are drawn from the buffer uniformly. This approach eliminates the correlations between the samples used in training the neural network and gives i.i.d. samples

The second feature that a DQN introduces is the target network. When bootstraping with function approximations, in a sense we create a moving target to learn from. Attempting to train a neural network via such a route is more likely bound to fail. The key idea is to create a copy of the neural network that is only used to generate the Q-value estimates used in sampled Bellman updates. That is the target value for sample $i$ is obtained as 

$$y_i = r_i + \gamma max_{\alpha_i} Q_{\theta_{TN}}(s_i, \alpha_i)$$

Note that in the update rule above, $\theta_{TN}$ denotes the parameters of a target network. These are updated every $C$ steps by setting them equal to the parameters $\theta$ i.e. $\theta_{TN} = \theta$. Such an update rule, creates a lag in updating the target network which may make the action-value estimates that it generates a bit stale compared to the original network. What we gain, however, is that the target values become stable and the original network is trainable.

Loss function
------

In the DQN algorithm, we use the following loss function

$$L = \begin{cases} \left( Q(s, \alpha) - (r + \gamma max_{\alpha \in \mathbb{A}}\hat{Q}(s_{NEW}, \alpha) \right)^2, \text{if step is not at the end of the episode} \\ \left( Q(s, \alpha) - r \right)^2, \text{otherwise}\end{cases}$$

The following section summarizes the DQN algorithm

DQN algorithm
------

1. Initialize $\theta$ and the replay buffer with a fixed capacity. Set $\theta_{TN}= \theta$
2. Set the policy $\pi$ to be an $\epsilon-$greedy with respect to $q_{\theta}$
3. Until some condition is met do

    3.1 Sample an action $\alpha$ from the policy $\pi$
    
    3.2 Take the action $\alpha$ and observe $r$ and $s_{NEW}$. Add the transition $(s, \alpha, r, s_{ NEW})$ in the replay buffer. If $|D|>M$ eject the oldest transition from the buffer
    
    3.3 If the experience buffer has reached the indicated capacity, unfiromly sample a random minibatch of $N$ transitions from $D$ else return to 3.1 above.
    
    3.4 Obtain the target values $y_i = r_i + \gamma max_{\alpha_i} q_{\theta_{TN}}(s_i, \alpha_i)$. 
    
    3.5 Take the gradient step to update $\theta$
    
    3.6 Every $C$ steps update the target network parameters

In a future post, we will implement a DQN for training on the OpenAI ```CartPole-v0``` environment.

References
======

1. Enes Bilgin, ```Mastering Reinforcement Learning with Python. Build next-generation, self-learning models using reinforcement learning techniques and best practices.```
2. Mnih V. et al. ```Human level control through deep reinforcement learning```, Nature, v. 518, pp. 529-533, 2015
3. Maxim Lapan, ```Deep Reinforcement Learning Hands-on```, Packt
