# Atari Breakout

# 1. Background and Problem Definition

Atari Breakout is a reinforcement learning task in which an agent controls a paddle to bounce a ball and destroy bricks. The agent observes the game screen and selects actions to maximize cumulative rewards.

Reinforcement Learning (RL) enables an agent to learn a control policy through interactions with an environment. In this experiment, Asynchronous Advantage Actor-Critic (A3C) is used to learn a gameplay strategy through parallel environment interactions.

The objective is to maximize cumulative rewards by learning to control the paddle and keep the ball in play while breaking bricks.

# 2. Algorithm Description: Asynchronous Advantage Actor-Critic (A3C)

A3C combines policy optimization with value estimation using two components: an Actor and a Critic. The Actor learns which actions to take, while the Critic estimates the expected cumulative reward associated with the current state.

## 2.1. State Space and Action Selection

The state consists of four consecutive grayscale frames, resized to 84 × 84 pixels and stacked to capture temporal information about the game.

The state representation is:

$$
s_t \in \mathbb{R}^{4 \times 84 \times 84}
$$

The Actor outputs a probability distribution over available actions:

$$
\pi(a_t \mid s_t; \theta)
$$

The agent selects an action according to the policy and receives a reward and the next state from the environment.

The neural network consists of three convolutional layers, a fully connected layer with 512 neurons, and separate Actor and Critic output layers.

## 2.2. Advantage Estimation

The advantage function measures whether an action produces a better or worse outcome than expected under the current value estimate.

$$
A_t = R_t - V(s_t)
$$

The multi-step return is calculated as:

$$
R_t = \sum_{k=0}^{n-1}\gamma^k r_{t+k}
+\gamma^n V(s_{t+n})
$$

Here, \(r_t\) represents the immediate reward, \(\gamma\) is the discount factor, and \(V(s_t)\) is the Critic's state-value estimate.

The advantage guides policy updates by evaluating the relative quality of the selected action.

## 2.3. Policy Loss and Value Loss

A3C optimizes the Actor and Critic using separate loss terms.

The policy loss encourages actions with positive advantages:

$$
L_{\mathrm{policy}}=-\log\pi(a_t\mid s_t)A_t
$$

The value loss measures the difference between the predicted state value and the target return:

$$
L_{\mathrm{value}}=(R_t-V(s_t))^2
$$

Entropy regularization encourages exploration by discouraging premature convergence to a deterministic policy.

The combined objective is:

$$
L=L_{\mathrm{policy}}+
c_vL_{\mathrm{value}}-
c_eH(\pi)
$$

where \(c_v\) and \(c_e\) control the contributions of the value loss and entropy regularization.

## 2.4. Asynchronous Training

A3C uses multiple workers to interact with separate environment instances. Each worker collects a sequence of transitions and calculates gradients from its local experience.

The gradients are applied to the global model, allowing workers to contribute experiences from different trajectories. This parallel training approach can improve experience diversity and reduce temporal correlation compared with a single sequential interaction stream.

## 2.5. Training Configuration and Evaluation

The training configuration is summarized below.

| Parameter | Value |
|---|---:|
| Environment | ALE/Breakout-v5 |
| Algorithm | A3C |
| Episodes | 10,000 |
| Number of workers | 4 |
| Rollout steps | 5 |
| Discount factor (\(\gamma\)) | 0.99 |
| Learning rate | 0.0007 |
| Entropy coefficient | 0.01 |
| Value loss coefficient | 0.5 |
| Maximum gradient norm | 40 |
| Input frame size | 84 × 84 |
| Number of stacked frames | 4 |
| Fully connected layer | 512 neurons |
| Optimizer | Adam |

During training, the following metrics are recorded:

- Episode reward: Total cumulative reward obtained in each episode.
- Policy loss: Measures the policy optimization objective.
- Value loss: Measures the difference between predicted state values and target returns.
- Entropy: Measures the degree of stochasticity in the action policy.

The reward reflects the agent's actual gameplay performance, while the loss tracks the optimization process. A decreasing loss does not necessarily indicate improved gameplay, so both metrics should be interpreted together.

# 3. Conclusion

## Atari Breakout Video

### Episode 1 vs. Episode 10000

The comparison video places Episode 1 on the left and Episode 10000 on the right to visualize behavioral differences between the initial and later training stages.


https://github.com/user-attachments/assets/6372b71f-b222-41e6-91eb-0f172e9aca6c


The comparison provides a qualitative view of the agent's gameplay behavior. Changes in paddle movement and ball interaction can be inspected to assess whether the learned policy exhibits improved control. However, visual differences alone do not establish superior performance; episode rewards and repeated evaluation trials are needed for quantitative comparison.

## Reward vs. Loss

Reward measures the agent's cumulative performance in the environment. Higher rewards generally indicate that the agent has maintained gameplay for longer or destroyed more bricks, depending on the environment's reward definition.

Loss measures the optimization objective used to update the Actor and Critic. A decreasing loss may indicate improved fitting to the learning targets, but does not necessarily imply higher rewards.

## Significance

Reward and loss provide complementary information about the training process. Reward evaluates gameplay performance, while loss helps track optimization behavior. Monitoring both metrics makes it possible to distinguish changes in actual performance from changes in the learning objective.

# 4. Conclusion and Significance

This experiment implements A3C for Atari Breakout by combining convolutional visual feature extraction, policy optimization, value estimation, and parallel environment interaction.

The Actor learns action-selection behavior, while the Critic estimates expected returns. Advantage estimation guides policy updates, and entropy regularization supports exploration.

Comparing gameplay videos across training stages provides an intuitive way to inspect behavioral changes. Combined with reward and loss tracking, this approach provides a practical foundation for understanding actor-critic reinforcement learning and comparing A3C with other algorithms, including DQN and A2C.

https://github.com/user-attachments/assets/08ef02df-de2e-4bd0-9417-6615ed213b9c



https://github.com/user-attachments/assets/830b24d3-3a7d-40a2-a869-25817f883019

