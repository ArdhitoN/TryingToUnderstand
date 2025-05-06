# Reinforcement Learning

1. In a discounted reward scheme, how to tune gamma to attain Blackwell optimality criterion?

Notes: Blackwell optimality criterion is the most selective criterion for valuing policy in infinite-horizon (infinite time), finite MDP (finite states and actions). It states: for gamma > gamma_bw, then the optimal policies will be the same.

One paper that talks about this : [Reducing Blackwell and Average Optimality to Discounted MDPs via the Blackwell Discount Factor](https://arxiv.org/abs/2302.00036)
- The authors explained that we can find the upper gamma Blackwell analytically. Therefore, we can take a slightly higher value from that upper gamma Blackwell as the discount factor gamma in our MDP.
- Also, the authors noticed a counterexample where at gamma > gamma_bw (with the classical definition), it doesn't always necessarily result in the same set of optimal policies. So, they formulate a new notation called gamma_upper_bw, which the set of optimal policies will then actually be the same across gamma > gamma_upper_bw. 

2. How do people tune gamma in practice?
In practice (realistic environment), I haven't seen any example that considers the Blackwell optimality criterion. Most of them rely on trying to get:
- The average episode reward. 
- Balancing the tradeoff between convergence time vs horizon considerations
  - Bigger gamma slows down convergence, but consider a longer horizon
  - Lower gamma faster convergence, but consider shorter horizon, thus actually could lead to suboptimal policy according to the most selective criterion, which is Blackwell.

Examples:
- [Hyperparameter Optimization for Reinforcement Learning using Meta’s Ax](https://www.digikey.com/en/maker/projects/hyperparameter-optimization-with-meta-ax/f09c53489dc94f13bcbcc2b6227c2ed4)
  
  - That blog + video demonstrates an interesting process of doing RL + hyperparameter tuning on the inverted pendulum task.
  - They use average episode reward as the objective function.
  - For hyperparameter tuning, they use Bayesian optimization with Meta's Ax Library.
  - The result of the agent seems quite good (with the final reward of -127, it seems can kind of balance the pendulum inverted), but of course could be improved. The author reflects that it might happen because the hyperparameter was stuck in local optima, and suggests to do a random search prior to Bayesian optimization to get better hyperparameters.
  - Another interesting observation is that after > 60k-70k timesteps, the train/std (refers to standard deviation for the noise when using generalized State Dependent Exploration) blows up, which likely makes a computation error, then a value error. So, be careful when training in RL; there could be many factors influencing an error, from vanishing or exploding gradients, computation error, etc.

  
