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


- [Optimizing hyperparameters of deep reinforcement learning for autonomous driving based on whale optimization algorithm
](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0252754)
- [HYPERPARAMETER TUNING FOR DEEP REINFORCEMENT
LEARNING APPLICATIONS](https://arxiv.org/pdf/2201.11182)
  - Uses genetic algorithm, comparing with bayesian approach
- [Meta-Gradient Reinforcement Learning](https://arxiv.org/pdf/1805.09801)
- [How to Discount Deep Reinforcement Learning:
Towards New Dynamic Strategies](https://arxiv.org/pdf/1512.02011)


3. If we want to evaluate a policy w.r.t Blackwell optimality criterion with discounting-free mechanisms, and say that it is for any unichain environment, can we just compare the gain + bias between policies to determine which one is better? Or must we approach the problem layer by layer, i.e., find the most optimal policy w.r.t gain, then find the most optimal policy w.r.t bias?
If we take a look at the Laurent series expansion of v_gamma, i.e.:
v_gamma = 1/(1 - gamma) * v_-1 + v_0 + e(pi, gamma) 

Seems it can be. Note that v_-1 = gain, and v_0 = bias.

But if we take a look from nBw policy gradient formulation that proposed in this paper: [A nearly Blackwell-optimal policy gradient method](https://arxiv.org/abs/2105.13609), they uses 2-level optimization process, i.e., finding the set of policies that gain optimal, then that result is become an input for the next selection, which tries to find the most optimal policies w.r.t bias value. 


4. What is fisher information and its correlation with natural gradient?
  
5. Why using linear parameterization and mean squared error emerge natural gradient, then that emergence could reduce the problem of policy improvement into regression?

6. Conceptually, in RL agent can't know the environment dynamics, but in many practical implementations, why do people put env as the agent attribute? Doesn't it make the agent can know any property of the environment?

7. What is ergodicity and how does it happen in MDP?

8. What's the limitations of Policy Gradient methods? 

9. What are good examples for Reinforcement Learning Environment with Transient States?
   
10. Is there any correlation between Modeling & Simulation with Reinforcement Learning? How could we have RL as a field?

11. How are gradient updates done in practice? Is it sampled properly from the real distribution?


