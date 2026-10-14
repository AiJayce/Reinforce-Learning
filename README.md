# Introduction

<img width="1025" height="415" alt="image" src="https://github.com/user-attachments/assets/af7c6c92-9ff5-4f8e-bda4-83f193dc75dd" />

Reinforcement Learning (RL) gained widespread public attention in 2016 when Google DeepMind’s AlphaGo defeated professional Go player Lee Sedol. By combining deep neural networks with reinforcement learning, AlphaGo learned to select promising moves in a game with an enormous number of possible states. This achievement demonstrated the potential of reinforcement learning to solve complex decision-making problems through trial and error, even when explicit correct answers are not available for every decision.

Reinforcement learning is a machine learning approach in which an agent interacts with an environment, selects actions, and learns a policy that maximizes cumulative rewards over time. Unlike supervised learning, which relies on labeled examples, reinforcement learning uses reward signals to evaluate the outcomes of actions and improve future decisions. The agent observes the current state, selects an action, receives a reward, and updates its policy based on the resulting experience. The value function estimates the expected future cumulative reward from a given state, while the Q-function estimates the expected cumulative reward of taking a specific action in that state. Representative algorithms include Deep Q-Network (DQN), which learns action values using neural networks; Asynchronous Advantage Actor-Critic (A3C), which combines policy learning with value estimation; and Proximal Policy Optimization (PPO), which stabilizes policy updates.

A key characteristic of reinforcement learning is its ability to optimize a sequence of decisions by considering not only immediate rewards but also their long-term consequences. This capability extends beyond games and robotics to potential applications in bioinformatics, where biological systems involve complex state transitions and interdependent processes. For example, in single-cell RNA sequencing analysis, gene expression profiles could be defined as states, and transitions between cellular states could be modeled as actions. By incorporating transcriptional continuity, transition directionality, and biological constraints into a reward function, reinforcement learning could be used to explore potential cellular transition trajectories. Similarly, it could be applied to gene regulatory network analysis to investigate how perturbations of specific genes influence downstream gene expression and cellular states.

While studying reinforcement learning, I became particularly interested in its potential applications in bioinformatics. Although reinforcement learning has already been applied to certain areas of life sciences, including drug discovery and protein design, its potential in single-cell transcriptomics, gene regulatory network inference, and cellular differentiation trajectory analysis remains worth exploring. By incorporating biological knowledge and analytical objectives into reward functions, reinforcement learning could complement conventional statistical methods and supervised learning approaches. For instance, an agent could explore cellular state transitions while accounting for transcriptional continuity and biological constraints, or evaluate the downstream effects of gene perturbations through sequential decision-making. Such applications would require carefully designed reward functions, rigorous comparisons with existing methods, and independent biological validation. Nevertheless, I believe that adapting the decision-making and trajectory-search capabilities of reinforcement learning to biological data could open up promising new directions in bioinformatics research.

# R-L practice

To establish a foundation in reinforcement learning (RL) for its application to bioinformatics, two tutorials were conducted using different RL algorithms: DQN and A3C. Each tutorial explored how an agent learns through states, actions, and rewards.

## 1. **CartPole — Deep Q-Network (DQN)**
   - State: Cart position and velocity, and pole angle and angular velocity.
   - Action: Move the cart to the left or right.
   - Reward: Receive a positive reward for each time step the pole remains balanced.
   - Algorithm: DQN uses a deep neural network to estimate the Q-value of each action in a given state and selects actions to maximize cumulative future rewards.

## 2. **Atari Breakout — Asynchronous Advantage Actor-Critic (A3C)**
   - State: The game screen, including the ball, paddle, and remaining bricks, represented as image frames.
   - Action: Move the paddle or perform other available game actions.
   - Reward: Gain positive rewards by breaking bricks and maximize the score while avoiding losing the ball.
   - Algorithm: A3C combines an Actor, which learns a policy for selecting actions, and a Critic, which estimates the state value. Multiple agents interact with the environment in parallel and asynchronously update shared model parameters to improve learning efficiency.
