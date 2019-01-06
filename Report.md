
## Report

I have implemented DDPG which is an off policy, actor critic method which uses ideas from [DQN](https://www.nature.com/articles/nature14236) and Deterministic Policy Gradients [DPG](http://proceedings.mlr.press/v32/silver14.pdf) to solve continuous action domains / spaces.

## Model Architecture

DDPG has Actor Network (Local and Target) which deterministically estimates the action and Critic Network (Local and Target) which takes the deterministic action to estimate Q Value respectively. 

Actor Network ==> States --> FC1(state_size * 256) --> FC2 (256 * 128) --> FC3 (128 * action_size).
    RELU is used for the Non Linearity, for the initial layers, and TANH is used in the final layer since every entry in the action vector should be a number between -1 and 1.

Critic Network ==> States ==> FCS1(state_size * 256) --> FC2(fcs1_units+action_size,128) --> FC3(128, 1)

Final Layer weights of both the Actor and Critic are initialized from a uniform distribution (-3e-3, 3e-3) and all the other layers are initialized from uniform distributions (-1/sqrt(f), 1/sqrt(f)) where f is the fanin of the layer

## Hyper Parameters

BUFFER_SIZE = int(1e6)  # replay buffer size
BATCH_SIZE = 128        # minibatch size
GAMMA = 0.99            # discount factor
TAU = 1e-3              # for soft update of target parameters
LR_ACTOR = 1e-4         # learning rate of the actor 
LR_CRITIC = 1e-4        # learning rate of the critic
WEIGHT_DECAY = 0        # L2 weight decay

Using [DDPG](https://arxiv.org/pdf/1509.02971.pdf), I am able to Solve Reacher Environment's (both Single and Multi). 

Environment Multi agents converged in 116 episodes with an average reward of 30.276 across 20 agents , whereas the single agent converged in 455 episodes with an average reward of 30.192.

Ornstein-Uhlenbeck process is used for introducing noise in the actions, but the values are clipped between -1 and 1.

[//]: # (Image References)

[image1]: https://github.com/abilashamarthaluri/continuos_control/blob/master/images/multi_reacher.jpg "Report for Multi Agent"

## Rewards Plot: 

[Rewards Plot][image1]

## Ideas for Future Work

There are several other algorithms proposed for solving continuos control problem's and have shown to be providing stable learning and faster convergance like [PPO](https://arxiv.org/abs/1707.06347) and [A3C](https://arxiv.org/abs/1602.01783) . I would like to explore and implement those algorithms to see how those algorithms perform. These is also some work on adding noise to the parameters instead of the action space by [Open AI](https://blog.openai.com/better-exploration-with-parameter-noise/) which allows for Better Exploration. Would also like to explore the effect of noise decay over time. 