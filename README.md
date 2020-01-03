## Several Reinforcement models
![rl_methods](/pics/rl.png)
#### 0. Q-tables, [ref](https://github.com/awjuliani/DeepRL-Agents/blob/master/Q-Table.ipynb), [code](https://github.com/ShangtongZhang/reinforcement-learning-an-introduction/blob/master/chapter01/tic_tac_toe.py)
    * Learn Q-tables with policy gradient
    * Bandit, tic_tac_toe examples
 
#### 1.1 Markov Decision Process, [code](https://github.com/ShangtongZhang/reinforcement-learning-an-introduction/blob/master/chapter03/grid_world.py)
    * MDP assumes a finite number of states (env) and actions (agent). 
    * Agent observes a state and executes an action, which incurs intermediate costs to be minimized.
    * The goal is to maximize the (expected) accumulated rewards.
    * Use Bellman equation to update each state value.
    
#### 1.2 Dynamic Programming, [code](https://github.com/ShangtongZhang/reinforcement-learning-an-introduction/tree/master/chapter04), [tutorial](https://www.analyticsvidhya.com/blog/2018/09/reinforcement-learning-model-based-planning-dynamic-programming/)    
    * Dynamic programming algorithms solve MDP as planning problems. 
    * Need to know entire env, can start at any state and learn next state.
    * Given a MDP, find an optimal policy for the agent to follow. It contains two main steps:
        a) Break the problem into subproblems and solve it
        b) Find overall optimal solution to the problem at hand
    * Usually contains 1) policy iteration 2) value iteration
    
#### 1.3 Monte Carlo, [code](https://github.com/ShangtongZhang/reinforcement-learning-an-introduction/blob/master/chapter05/blackjack.py), [tutorial](https://oneraynyday.github.io/ml/2018/05/24/Reinforcement-Learning-Monte-Carlo/)    
    * MC samples from the experience to estimate the env.
    * MC can also start at any state, but need take all possible actions at every start of an episode.
    * MC learns episode-by-episode.
    * Use Importance Sampling to learn the sampling strategy.
    * MC needs to wait until the final reward before any state-action pair values can be updated.
    * Once the final reward was received, the path taken to reach the final state would need to be traced back and each value updated accordingly.

#### 2. Temporal Difference, [code](https://github.com/ShangtongZhang/reinforcement-learning-an-introduction/tree/master/chapter06), [tutorial](https://www.cse.unsw.edu.au/~cs9417ml/RL1/tdlearning.html)
    * Temporal Difference (TD) Learning is used to estimate value functions. 
    * Unlike MC, TD estimates the final reward at each state and the state-action value updates for every step. 
    * TD is a combination of DP and MC.
    * On-Policy v.s. Off-Policy Learning:
        On-Policy TD can learn the value of the policy that is used to make decisions. 
    * Off-Policy TD can learn different policies for behaviour and estimation. 
        Behaviour policy is usually "soft" so there is sufficient exploration going on.
    * TD(0) - learn state-value function v(s)
    # Sarsa(On-policy) - learn an action-value function q(a,s)
    # Q-learning(Off-policy) - learn action-value regardless of selected action
    # Act-critic - critic is mearsuing V(s), actor is an independent policy. Use actor to choose action, and use TD to update critic. 
    
    
#### 3. Q-learning
    * Dyna-Q
        a) Integrating planning, acting, and learning
    * DQN
        a) Use separate networks for 1) updating weights and 2) generate current decisions
        b) Use replay memory to sample from so that the training process is stable
        c) Use epsilon-greedy to sample actions from predicted distribution
        d) DDPG is DQN + policy learning

#### 4. Policy Gradient, [DDPG](https://arxiv.org/pdf/1509.02971.pdf), [TD3](https://spinningup.openai.com/en/latest/algorithms/td3.html)
    * Previous methods select actions based on estimated action values.
    * Here we learn a parameterized policy that can select actions without consulting a value function.
    * 4.1 Standard PG
    * 4.2 REINFORCE
    * 4.3 Deep Deterministic Policy Gradient (DDPG), see 5.5
        a) DDPG is an off-policy algorithm, which is used for environments with continuous action spaces.
        b) Concurrently learn a Q-function and a policy, which can be thought of deep Q-learning for continuous action spaces.
        c) DDPG interleaves learning an approximator to Q(s,a) with learning an approximator to a(s).
        d) For Q-learning side, it minimizes MSE of target Q and pred Q.
        e) For policy gradient side, it learns a deterministic policy which gives the action that maximizes Q by gradient ascent.
     * 4.4 Twin Delayed DDPG (TD3), see 5.6
         a) TD3 learns two Q-functions and uses the smaller of the two Q-values to form the targets in the Bellman error loss functions.
         b) Delayed Policy Updates. TD3 updates the policy (and target networks) less frequently than the Q-function.
         c) Target Policy Smoothing. TD3 adds noise to the target action, to make it harder for the policy to exploit Q-function errors by smoothing out Q along changes in action.
    
#### 5. Actor-Critic, [A2C](https://www.freecodecamp.org/news/an-intro-to-advantage-actor-critic-methods-lets-play-sonic-the-hedgehog-86d6240171d/), [A3C](https://medium.com/emergent-future/simple-reinforcement-learning-with-tensorflow-part-8-asynchronous-actor-critic-agents-a3c-c88f72a5e9f2),  [PPO](https://arxiv.org/pdf/1707.06347.pdf), [SAC](https://arxiv.org/pdf/1812.05905.pdf)
    Policy gradient method
    * 5.1 A2C
        a) Advantage Actor-Critic RL
        b) Train Actor and Critic networks
        c) Define worker function which has independent gym environment, and simulates CartPole
        d) Creates multiple processes for workers to update networks
    * 5.2 Continuous A3C
        Continuous Asynchronized Actor Critic
    * 5.3 Discrete A3C
        Discrete Asynchronized Actor Critic
    * 5.4 Proximal Policy Optimization (PPO)
    * 5.5 DDPG with AC formulation (see 4.3), with Q-leearning (value) + Actor-Critic (policy) learning
    * 5.6 TD3 with AC formulation (see 4.4), with Q-learning (value) + Actor-Critic (policy) learning
    * 5.7 Soft AC (SAC)
        a) off-policy maximum entropy
        b) successor of Soft Q-Learning and incorporates the double Q-learning trick similar as TD3. 
        c) maximize a trade-off between expected return and entropy, a measure of randomness in the policy
        d) policy is parameterized as a neural network

#### 8 Distributional Quantile-DQN, [C51](https://arxiv.org/pdf/1707.06887.pdf), [QR-DQN](https://arxiv.org/pdf/1710.10044.pdf), [IQN](https://arxiv.org/pdf/1806.06923.pdf)
  ![Network](/pics/iqn.png)
  
    * 8.1 C51
        a) Define distributional Bellman equation which approximates value distributions.
        b) Value distribution Z_pi is a mapping from state-action pairs to distributions over returns.
        c) The distributional Bellman operator preserves multimodality in value distributions, which leads to more stable learning.
        d) Perform a heuristic projection step, followed by the minimization of a KL divergence between projected Bellman update and prediction.
        e) Fail to propose a practical distributional algorithm that operates end-to-end on the Wasserstein metric.
    * 8.2 QR-DQN
        a) Distributional reinforcement Learning with quantile regression.
        b) By using quantile regression (Koenker 2005), there exists an algorithm  which can perform distributional RL over the Wasserstein metric.
        c) Assign fixed uniform probabilities to N adjustable locations.
        d) Stochastically adjust the distributions’ locations so as to minimize the Wasserstein distance to a target distribution.
    * 8.3 Implicit-Quantile-DQN (IQN)
        a) Implicit Quantile Networks for Distributional Reinforcement Learning
        b) Use continuous quantile estimation to predict state distribution
        c) Use TD to update according to p-Wasserstein distance
    