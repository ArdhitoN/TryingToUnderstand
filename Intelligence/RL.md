# Reinforcement Learning

1. In a discounted reward scheme, how to tune gamma to attain Blackwell optimality criterion?
Notes: Blackwell optimality criterion is the most selective criterion for valuing policy in infinite-horizon (infinite time), finite MDP (finite states and actions). 
One paper that talks about this : [Reducing Blackwell and Average Optimality to Discounted MDPs via the Blackwell Discount Factor](https://arxiv.org/abs/2302.00036)

The authors explained that we can find the upper gamma Blackwell analytically. Therefore, we can take a slightly higher value from that upper gamma Blackwell as the discount factor gamma in our MDP. 

