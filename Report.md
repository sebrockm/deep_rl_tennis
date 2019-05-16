# Report

## Algorithm
Hyperparameters and special algorithmic features are printed in **bold**.

This project uses a Deep Deterministic Policy Gradients (DDPG) algorithm
That means two neural networks are used:
 1. The actor approximates the Q-table and thus predicts the best action given a state.
 2. The critic predicts the maximum possible target that the action predicted by the actor will grant for the given state.
    This target drives the learning for both, the critic as well as the actor.

For details on the neural network architecture see the section below.

The algorithm runs for at most **`n_episodes = 2000`** episodes.
In each episode the environment is reset and then actions are applied until the environment yields that it is done.
In each timestep for each agent an action is generated based on the weights of the current actor network.
This means, the current policy is applied.

Some noise is added to the action in order to randomly alter it.
This allows to further explore the action space.
Then, the action is applied in the environment and the corresponding award and new state are observed.
The corresponding `(state, action, reward, next_state, done)` tuple is enqueued into an **Experience Replay** buffer which has a maximum length of **`replay_buffer_size = 100000`**. 
The idea is to have a limited memory of past experiences that we can learn from again.
Since it's limited, the quality of the stored experiences will increase over time.

A random sample of size **`batch_size = 128`** is drawn from the experience replay buffer.
The algorithm uses two identical networks of each, the actor and the critic, to apply the **Fixed Q-Targets** technique.
This technique decuples the input wheights from the wheight adjustment due to learning:
The "next states" (s') of the sample batch are fed into the secondary ("fixed") actor network.
The resulting action, together with the next state, are fed into the secondary ("fixed") critic network.
Together with the reward and an applied discount rate of **`gamma = 0.99`**, this gives the target values.

The primary critic network is fed with the batch of current states and actions.
This result and the previously generated target values are passed to the loss function (**medium quared error** is used in this project) to calculate an error for the critic.
This error is used to calculate the gradients needed to update the weights of the primary critic network.
The learning rate was chosen to be **lr=0.0001**.
Then, the primary actor network is fed the batch of current states and the result is again passed, together with the states, to the primary critic.
The result is used as the primary actor's loss function that is needed for the gradients to update the weights of the primary actor network.
The learning rate here was also chosen to be **lr=0.0001**.

Lastly, the wheights of the fixed networks are actually not fixed, but are slightly updated with the new wheights of the primary networks.
The factor of **`tau = 0.1`** denotes the portion of the new weights that are interpolated into the old ones.

### The neural network architectures
The actor network used in this solution has three layers:
  1. A fully connected layer with 512 units
  2. A fully connected layer with 256 units
  3. An output layer with 2x2 = 4 units, two action variables for each agent
  
Between the layers 1 and 2 and between the layers 2 and 3 ReLU activation is applied.
The final activation function is `tanh` whose output is always between `-1` and `1`, just as needed for the given action space.

The critic network also three layers:
  1. A fully connected layer with 512 units
  2. A fully connected layer with 256 units
  3. An output layer with 1 unit
  
The critic also feeds the states of the agents into the first layer and applies ReLU, 
but then the output of the first layer is concatenated with the actions before it is fed into the second layer.
Then, again ReLU is applied and the result is passed to the last layer.
Here, no activation function is applied, because the critic must generate an arbitrary number.

### Training progress
![graph](scores.png?raw=true "scores")

## Output
Episode 100	Average Score: 0.0048
Episode 200	Average Score: 0.0166
Episode 300	Average Score: 0.0966
Episode 400	Average Score: 0.1245
Episode 500	Average Score: 0.1498
Episode 600	Average Score: 0.1479
Episode 700	Average Score: 0.2049
Episode 800	Average Score: 0.2390
Episode 900	Average Score: 0.2607
Episode 1000	Average Score: 0.2708
Episode 1100	Average Score: 0.3417
Episode 1200	Average Score: 0.3395
Episode 1300	Average Score: 0.4676
Episode 1340	Average Score: 0.4984
Environment solved in 1341 episodes!	Average Score: 0.5054
Episode 1400	Average Score: 0.4635
Episode 1500	Average Score: 0.3647
Episode 1600	Average Score: 0.4285
Episode 1700	Average Score: 0.2715
Episode 1800	Average Score: 0.4328
Episode 1900	Average Score: 0.3368
Episode 2000	Average Score: 0.3652
Episode 2042	Average Score: 0.3641

So, the goal is reached after ~1300 episodes.
Then, the performance decreases again.

## Possible future Enhancements
In general, very many different combinations of the hyperparameters can be tested and evaluated.
As part of this, also different network architectures can be used.
Also, other actor critic methods could be given a try like:
 - A3C
 - A2C
 - GAE
