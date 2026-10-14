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

### Page at : https://github.com/AiJayce/Reinforce-Learning/tree/Cart-pole

## 2. **Atari Breakout — Asynchronous Advantage Actor-Critic (A3C)**
   - State: The game screen, including the ball, paddle, and remaining bricks, represented as image frames.
   - Action: Move the paddle or perform other available game actions.
   - Reward: Gain positive rewards by breaking bricks and maximize the score while avoiding losing the ball.
   - Algorithm: A3C combines an Actor, which learns a policy for selecting actions, and a Critic, which estimates the state value. Multiple agents interact with the environment in parallel and asynchronously update shared model parameters to improve learning efficiency.

### Page at : https://github.com/AiJayce/Reinforce-Learning/tree/Breakout

# Applications in Bioinformatics

<img width="1024" height="434" alt="image" src="https://github.com/user-attachments/assets/300c99e0-7007-4a5a-ad6e-45555d939e1e" />

Although applications of reinforcement learning (RL) in bioinformatics remain relatively limited, RL has significant potential to learn optimal strategies for achieving predefined objectives in environments where explicit ground-truth labels are unavailable. Supervised learning focuses on predicting predefined labels, while unsupervised learning aims to identify the underlying distributions and structural patterns of data. In contrast, RL enables an agent to learn from rewards associated with state transitions through interactions with an environment and optimize its policy to maximize long-term cumulative rewards.

Biological systems are dynamic, continuously changing in response to gene expression, cellular differentiation, external stimuli, and microenvironmental conditions. These characteristics align with the RL framework, which is based on state transitions and long-term reward optimization. By leveraging this framework, it may be possible to move beyond analyzing static similarities between cellular states and explore the directionality of state transitions and potential cellular trajectories.

First, a reward function designed around biological objectives can provide a framework for learning dynamic cellular transitions without explicit transition labels. For example, gene expression profiles or positions in a latent space can be defined as states, with rewards assigned to transitions that approach a specific biological objective. This enables the exploration of potential transition trajectories toward a target state.

Second, the value function can account for expected long-term cumulative rewards from the current state and subsequent transitions. This enables the comparison of potential transition pathways and the analysis of biological processes with multiple possible trajectories, such as cellular differentiation and disease progression. Furthermore, incorporating continuity in the state space and similarity between transitions may help model gradual changes in cellular states.

Third, the exploration–exploitation mechanism of RL enables the exploration of diverse candidate pathways that align with a reward function, rather than being restricted to patterns observed in existing data. This approach may facilitate the generation of hypotheses regarding novel transition pathways in biological processes where the sequence of transitions is not clearly established, including cellular differentiation, immune cell activation, and disease progression.

Furthermore, modeling cellular state transitions based on gene expression can reveal associations between changes in gene expression and transition trajectories. Integrating perturbation data, such as gene knockouts or drug treatments, may extend this approach to investigating how specific interventions influence cellular state transitions.

Building on these possibilities, the following projects were conducted to establish a foundational understanding of RL and investigate its potential application to bioinformatics, particularly in exploring dynamic changes in cellular states and potential transition trajectories.
