# Cart - pole

# 1. Background and Problem Definition

CartPole is a control problem in which an agent must balance a pole attached to a cart moving along a horizontal track. The agent observes the cart's position and velocity, as well as the pole's angle and angular velocity, and applies force to move the cart either left or right.

Reinforcement Learning (RL) enables an agent to learn a control policy through interactions with an environment, aiming to maximize cumulative rewards. In this experiment, Deep Q-Network (DQN) is used to learn a balancing strategy without explicitly defining control rules.

The CartPole-v1 environment provides a reward of +1 for each time step during which the pole remains balanced. The objective is to maximize cumulative rewards by maintaining balance until the episode terminates or reaches the maximum time limit of 500 steps.

# 2. Algorithm Description: Deep Q-Network (DQN)

DQN combines Q-learning with deep neural networks to estimate the expected cumulative reward associated with each action in a given state. The agent learns to select actions with the highest estimated Q-values.

## 2.1. State Space and Action Selection

The state consists of four variables:

- Cart position
- Cart velocity
- Pole angle
- Pole angular velocity

The action space consists of two actions: moving the cart left or right. The neural network receives the current state as input and outputs a Q-value for each possible action.

$$
Q(s_t, a; \theta)
$$

Here, $s_t$ represents the current state, $a$ denotes an action, and $\theta$ represents the neural network parameters.

The implemented network consists of an input layer with four features, two hidden layers with 128 neurons each, and an output layer with two units.

$$
4 \rightarrow 128 \rightarrow 128 \rightarrow 2
$$

The output values represent estimated cumulative rewards rather than action probabilities. The agent selects the action with the highest Q-value.

## 2.2. Epsilon-Greedy: Exploration and Exploitation

During early training, the network's predictions may not reliably identify the optimal action. The Epsilon-Greedy strategy balances exploration and exploitation.

- **Exploration:** Select a random action with probability $\epsilon$ to collect diverse experiences.
- **Exploitation:** Select the action with the highest Q-value with probability $1-\epsilon$.

In this experiment, epsilon starts at 1.0 and is multiplied by 0.995 after each episode, with a minimum value of 0.05. This encourages broad exploration during early training and increasingly favors learned actions as training progresses.

## 2.3. Experience Replay: Reusing Past Experiences

During interaction with the environment, the agent collects experiences represented as:

$$
(s_t, a_t, r_t, s_{t+1}, d_t)
$$

These components represent the current state, selected action, reward, next state, and episode termination indicator, respectively.

Experiences are stored in a replay buffer with a maximum capacity of 50,000 transitions. For each training update, 64 experiences are randomly sampled to construct a mini-batch.

Consecutive transitions are often correlated. Random sampling helps reduce this temporal correlation and allows past experiences to be reused multiple times, improving data efficiency. Training begins once the buffer contains at least 64 experiences.

## 2.4. Bellman Equation and Q-Value Updates

The central principle of DQN is to update the network's Q-value predictions toward reward-based target values.

$$
y_t = r_t + \gamma (1-d_t)
\max_{a'} Q(s_{t+1}, a'; \theta^-)
$$

Where:

- $r_t$: Immediate reward obtained from the current action
- $\gamma$: Discount factor, set to 0.99
- $d_t$: Episode termination indicator
- $\theta^-$: Parameters of the Target Network

The target Q-value combines the immediate reward with the maximum estimated future value of the next state, discounted by $\gamma$. If the episode terminates, the future-value term is excluded.

For example, when the pole remains balanced and the agent transitions to the next state, the target incorporates both the immediate reward and the expected future reward. When the episode terminates, the target is based only on the immediate reward.

The difference between the predicted Q-value and the target Q-value is known as the Temporal-Difference (TD) Error.

$$
\delta_t = y_t - Q(s_t, a_t; \theta)
$$

The network is trained to minimize this error, improving the accuracy of its Q-value estimates.

## 2.5. Loss Function and Network Optimization

The experiment uses Huber Loss to measure the difference between the predicted Q-value and the target Q-value.

$$
L_\delta(e) =
\begin{cases}
\frac{1}{2}e^2 & \text{if } |e| \leq \delta \\
\delta(|e|-\frac{1}{2}\delta) & \text{otherwise}
\end{cases}
$$

Here, $e$ is the difference between the target Q-value and the predicted Q-value, and $\delta$ is the threshold separating the quadratic and linear regions. The implementation uses the default threshold of 1.0.

Huber Loss behaves like squared error for small deviations and approaches absolute error for large deviations. This reduces the influence of unusually large prediction errors and helps stabilize optimization.

Backpropagation computes gradients of the loss with respect to the network parameters, and the Adam optimizer updates these parameters using a learning rate of 0.001. Gradient clipping is also applied to limit the gradient norm to 10, reducing the risk of excessively large updates.

## 2.6. Target Network: Stabilizing Learning

If the same network is used to generate both predictions and rapidly changing target values, training can become unstable. DQN addresses this issue by maintaining two networks.

- **Policy Network:** Predicts Q-values for the current state and is updated through backpropagation.
- **Target Network:** Estimates future Q-values used to construct training targets and is not directly updated through backpropagation.

In this implementation, the Target Network is synchronized with the Policy Network every 10 episodes.

$$
\theta^- \leftarrow \theta
$$

Keeping the Target Network fixed between synchronization steps reduces rapid fluctuations in the learning targets and helps stabilize training.

## 2.7. Training Configuration and Evaluation

The agent is trained for 1,000 episodes using the following configuration.

| Parameter | Value |
|---|---:|
| Episodes | 1,000 |
| Batch size | 64 |
| Replay buffer capacity | 50,000 |
| Discount factor ($\gamma$) | 0.99 |
| Learning rate | 0.001 |
| Initial epsilon | 1.0 |
| Minimum epsilon | 0.05 |
| Epsilon decay | 0.995 per episode |
| Target Network update | Every 10 episodes |
| Gradient clipping norm | 10 |
| Hidden layer architecture | 128 → 128 |

During training, the following metrics are recorded:

- Episode reward: Total cumulative reward obtained in each episode.
- Mean reward over the last 50 episodes: A measure of recent control performance.
- Average loss: The mean Huber Loss across training updates within an episode.
- Epsilon: The current exploration rate.

Policy Network checkpoints are saved at episodes 1, 100, 500, 750, and 1,000. These checkpoints allow the learned policy to be evaluated at different stages of training.

## 2.8. Policy Visualization

A random policy before training and the policies from the saved checkpoints are evaluated under the same initial evaluation conditions. Videos are generated to visualize changes in control behavior across training stages.

During evaluation, random exploration is disabled, and the agent consistently selects the action with the highest Q-value.

The videos provide a qualitative comparison of the learned policies, while cumulative rewards provide a quantitative measure of performance. Because evaluation episodes use a separate execution from training, evaluation rewards may differ from the rewards recorded during training.

# 3. Conclusion

## Cart-pole vieo

https://github.com/user-attachments/assets/d96be1b6-0f14-4015-b6fc-d6a50cd89f96

# 4. Conclusion and Significance

This experiment implements the core DQN learning cycle: state observation, action selection, reward collection, and iterative Q-value updates.

Experience Replay enables past experiences to be reused while reducing temporal correlation. The Target Network stabilizes target-value estimation, while Epsilon-Greedy balances exploration and exploitation. Huber Loss and gradient clipping further help control the optimization process.

By tracking cumulative rewards and training losses and comparing policy behavior across checkpoints, the experiment evaluates both learning progress and control performance.

This implementation provides a practical foundation for understanding value-based reinforcement learning and can serve as a baseline for comparing DQN with other reinforcement learning algorithms, such as Advantage Actor-Critic (A2C) and Asynchronous Advantage Actor-Critic (A3C).
