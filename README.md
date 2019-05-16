# deep_rl_tennis

DRL Agent to play tennis.


# Project: Tennis

### Intro

[//]: # (Image References)

[image1]: https://s3.amazonaws.com/video.udacity-data.com/topher/2018/May/5af7955a_tennis/tennis.png "Trained Agent"


This projects goal is to utilize Deep Reinforcement Learning (DRL) to train two agents to play tennis so that the ball keeps being in the game for as long as possible.

Trained agents  can be seen in below image: 

![Trained Agent][image1]

(source: https://s3.amazonaws.com/video.udacity-data.com/topher/2018/May/5af7955a_tennis/tennis.png)

## Environment
### Action space
At each timestep the agents have to choose a 2 dimensional continuous action, all values between `-1` and `1`: They encode the movement towards (or away from) the net and the jumping.

### State space

The state space is a continuous 8 dimensional vector that contains e.g. the position and velocity of the ball and the racket.

A reward of `+0.1` is provided whenever an agent hits the ball over the net.
A reward of `-0.01` is provided whenever the ball hits the ground or falls out of bounds.

The task is episodic. The problem is considered solved once the average score of the last 100 episodes is greater than `0.5` where each single score of an episode is considered the maximum of the two agents' scores.

## How to use

### Installing dependencies

- Download ML-Agents Toolkit beta 0.4.0a.
   - https://github.com/Unity-Technologies/ml-agents/releases/tag/0.4.0a
- install it by running following command, with activated conda environment in the directory of ml-agents, that contains the setup.py.
   - pip install -e . .
- install PyTorch.
    - conda install pytorch-cpu torchvision-cpu -c pytorch
- Get Unity Environment designed for this project.
   - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P3/Tennis/Tennis_Linux.zip).
    - Place the file in the DRLND GitHub repository, in the `deep_rl_tennis/` folder, and unzip (or decompress) the file. 

### Instructions

Follow the instructions in `Tennis.ipynb`.
Then, execute the cells to train the agent.

### Expected Result
After approximately 1300 episodes the agents reach the goal of a score of 0.5 on average.
If it runs further, the performance decreases, unfortunately.
