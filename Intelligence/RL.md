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
- [Automatic Hyperparameter Tuning - In Practice (Optuna)](https://araffin.github.io/post/optuna/)
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

- [HIGH-DIMENSIONAL CONTINUOUS CONTROL USING
GENERALIZED ADVANTAGE ESTIMATION](https://arxiv.org/pdf/1506.02438)

- [HYPERPARAMETER TUNING FOR REINFORCEMENT LEARNING WITH BANDITS AND OFF-POLICY SAMPLING](https://etd.ohiolink.edu/acprod/odb_etd/ws/send_file/send?accession=case1613034993418088&disposition=inline&utm_source=chatgpt.com)
- [Hyperparameter Selection in Reinforcement Learning Using the “Design of Experiments” Method](https://www.sciencedirect.com/science/article/pii/S1877050923009055/pdf?md5=4b3167166e6accc47dc92052724b028a&pid=1-s2.0-S1877050923009055-main.pdf)

Lecture on discounted PG & Natural PG: https://cs.stanford.edu/~amishkin/assets/slides/policy_gradients.pdf


Papers that doesn't tune gamma (use fixed value, e.g., 0.99): 
* [Asynchronous Methods for Deep Reinforcement Learning](https://arxiv.org/abs/1602.01783)
* [The AI Economist: Optimal Economic Policy Design via Two-level Deep Reinforcement Learning](https://arxiv.org/abs/2108.02755)
* [The Definitive Guide to Policy Gradients in Deep Reinforcement Learning: Theory, Algorithms and Implementations](https://arxiv.org/abs/2401.13662)

3. If we want to evaluate a policy w.r.t Blackwell optimality criterion with discounting-free mechanisms, and say that it is for any unichain environment, can we just compare the gain + bias between policies to determine which one is better? Or must we approach the problem layer by layer, i.e., find the most optimal policy w.r.t. gain, then find the most optimal policy w.r.t bias?
If we take a look at the Laurent series expansion of v_gamma, i.e.:
v_gamma = 1/(1 - gamma) * v_-1 + v_0 + e(pi, gamma) 

Seems it can be. Note that v_-1 = gain, and v_0 = bias.

But if we take a look from nBw policy gradient formulation that proposed in this paper: [A nearly Blackwell-optimal policy gradient method](https://arxiv.org/abs/2105.13609), they uses 2-level optimization process, i.e., finding the set of policies that gain optimal, then that result is become an input for the next selection, which tries to find the most optimal policies w.r.t bias value. 


4. What is fisher information and its correlation with natural gradient?
https://jonathan-hui.medium.com/rl-natural-policy-gradient-actor-critic-using-kronecker-factored-trust-region-acktr-58f3798a4a93
Natural --> utilize the structure of policy distribution, i.e., changes in policy parameter induces changes in the distribution of policy \pi. 
  
6. Why using linear parameterization and mean squared error emerge natural gradient, then that emergence could reduce the problem of policy improvement into regression?

7. Conceptually, in RL agent can't know the environment dynamics, but in many practical implementations, why do people put env as the agent attribute? Doesn't it make the agent can know any property of the environment?

8. What is ergodicity and how does it happen in MDP?
Ergodicity needs the MDP to be recurrent chain.

10. What's the limitations of Policy Gradient methods?
* Might not be working as expected (go to local optima instead of global optima)


10. What are good examples for Reinforcement Learning Environment with Transient States?

   
11. Is there any correlation between Modeling & Simulation with Reinforcement Learning? How could we have RL as a field?

12. How are gradient updates done in practice? Is it sampled properly from the real distribution?

13. Back to basic, as policy gradient is an optimization problem, what makes a valid optimization problem?
* Insight from: [Discounted Reinforcement Learning Is Not an Optimization Problem](https://arxiv.org/abs/1910.02140)

Optimization problem needs at least a set of feasible solutions and an objective function that produces a 
scalar value that represents each of the solution's value. 
In continuing task with RL setting, we need to use function approximation. 
With that, using discounted reward as the optimality criterion is not producing an optimization problem,
there's no clear objective function, 
since policy's value depends on initial state, 
hence different initial state could produce different optimal policies.
We could make the objective as a single number by making possible initial states as weights based on initial state distribution, 
but such approach might not be appropriate 
as the initial state might not represent the whole states that will be visited by the agent throughout the horizon.
Such objective could be made more sense through several ways, but it eventually leads to average reward optimality criterion.

13. How's the performance comparation between average reward vs discounted reward RL?
* One paper that talks about this: [Performance Bounds for Policy-Based Average Reward Reinforcement Learning Algorithms](https://arxiv.org/abs/2302.01450) 
  
14. How to stabilize avg reward based RL?
* One paper that talks about this: [Average-Reward Reinforcement Learning with Entropy Regularization](https://arxiv.org/abs/2501.09080) 

15. Where the stationarity refers to in stationary MDP? Is it changing in terms of the possible states and actions? 
* Insight from: [Non-Stationary Markov Decision Processes a Worst-Case Approach using Model-Based
Reinforcement Learning](https://arxiv.org/pdf/1904.10090)

Actually, the stationarity refers to just the state transition probability (P) and the reward function (r). I haven't seen any sources that say about changing state set and action set yet.

16. How's the best reward function formulations?
* Can we define more than 1 reward function to be maximized? For example to maximize one objective whilst minimizing risk?

Yes, there's a concept called constrained MDP: [Constrained MDPs and the reward hypothesis](https://readingsml.blogspot.com/2020/03/constrained-mdps-and-reward-hypothesis.html)


17. What's the difference between Deep Policy Gradients vs Non-Deep Policy Gradients?

* Insight from [Deep Policy Gradient Algorithms: A Closer Look](https://youtu.be/7J2zajQe7lw?si=KcETr_dkxCFOWUtS)
There are several problems with Deep RL:

a. Poor reliability over repeated runs (w/ different seeds)

b. High sensitivity to hyperparameters

c. Poor robustness to env artifacts

To get rid of that, let's get back to the first principles:

a. Gradient estimates --> take k-samples, measure gradient variance (mean pairwise correlation --> higher means lower concentration, whilst lower means worse concentration)

b. Value prediction --> gradient estimation hindered by high variance. Thus: 
- - if we can estimate value of a state, it can lower the variance. Intuition: separating the value of a sate with the action improve the accuracy of determining which one contributes more to the state-action value
  We can use Neural Network for estimating values.

c. Optimization Landscape
The key assumption is that: taking gradient steps increases overall reward
How this is valid in practice?
The reality is that, steps oftentimes not predictive in the optimization process. Why? Because it iteratively maximize the surrogate reward, not the true reward!

d. Trust Regions
-> The steps taken should be based on our samples. 
-> TRPO & PPO use KL Distance for this.

Recap: Deep RL training dynamics poorly understood:
- Steps are often uncorrelated
- Surrogate reward doesn't match the true rewards
- Trust Regions does not hold

To get rid of that, we need:
- Reconcile RL with conceptual framework
- Rethink primitives for modern settings (high dimensionality, non-convex function approximations, algorithm optimizations)
- Get better evaluations for RL systems (reliability, safety, robustness)


18. Is bias an optimality criterion?
Yes, we could consider bias as an optimality criterion as an extension of average reward optimality criterion, if we refer to the Veinott's.

19. What is geometric distribution?
* Ref: https://towardsdatascience.com/geometric-distribution-simply-explained-9177c816794f/
  * Number of Bernoulli trials to get a "success" outcome. Success here could refer to many things, depends on context.
  * Key properties:
    * Memoryless (each bernoulli trial is independent. Means failure in prev trial doesn't affect the next trial prob of succeeding)
    * 


20. Are Policy Gradient Methods Always Working?
* Insight from: [CS 182: Lecture 15: Part 3: Policy Gradients](https://www.youtube.com/watch?v=EsDm2K5nKeA), [Policy Gradient Methods: Tutorial and New Frontiers](https://www.youtube.com/watch?v=y4ci8whvS1E)
* To make it work, we can use a baseline as a variance reducer, that doesn't introduce bias.
* 


21. How to measure sample complexity of RL algorithm?
* Refs: https://ai.stackexchange.com/questions/38775/do-the-terms-sample-complexity-and-sample-efficiency-mean-the-same-thing-in, https://ai.stackexchange.com/questions/21992/how-to-measure-sample-efficiency-of-a-reinforcement-learning-algorithm/21996#21996, https://www.youtube.com/watch?v=DT2MbIeec4U, https://www.datacamp.com/blog/what-is-sample-complexity
* * We need to set the treshold value, then find how many samples needed for the algo to reach that treshold.
* * [An Improved Analysis of (Variance-Reduced) Policy Gradient and Natural Policy Gradient Methods](https://arxiv.org/pdf/2211.07937)
* * [A general sample complexity analysis of vanilla policy gradient](https://proceedings.mlr.press/v151/yuan22a/yuan22a.pdf)
* * [Improved Sample Complexity Analysis of Natural Policy Gradient Algorithm with General Parameterization for Infinite Horizon Discounted Reward Markov Decision Processes](https://proceedings.mlr.press/v238/mondal24a/mondal24a.pdf)
  * [Elementary Analysis of Policy Gradient Methods](https://arxiv.org/pdf/2404.03372)
  * [GLOBAL CONVERGENCE OF POLICY GRADIENT IN AVERAGE REWARD MDPS](https://openreview.net/pdf?id=2PRpcmJecX)
  * [Analysis of On-policy Policy Gradient Methods under the Distribution Mismatch](https://arxiv.org/html/2503.22244v1#:~:text=Theoretical%20analysis%20for%20tabular%20parameterizations,optimal%20solutions%20under%20tabular%20policies)
  * [Practical Principled Policy Optimization for Finite MDPs](https://openreview.net/pdf?id=c838OfxQwF)
  * [On the Convergence Rates of Policy Gradient Methods](https://arxiv.org/pdf/2201.07443)
  * [Regret Analysis of Policy Gradient Algorithm for Infinite Horizon Average Reward Markov Decision Processes](https://arxiv.org/pdf/2309.01922)
  * [A Sharper Global Convergence Analysis for Average Reward Reinforcement Learning via an Actor-Critic Approach](https://arxiv.org/pdf/2407.18878)
  * O(\epsilon^p) states how the amount of samples needed to improve the accuracy of the policy from the GO (Global Optimal) with improvement measure of \epsilon^p.

 
* * Things that correlated with sample complexity:
  * * Bias : in the form of approximate-based variables.
    * Variance
    * Exploration: the ideal condition is that samples include all possible state & action spaces, so the sample can be representative about the whole RL problem  (?)
    * Generalization

* Formally, we need to play with PAC concept https://en.wikipedia.org/wiki/Probably_approximately_correct_learning

* For comparing sample complexity between discounting-free polgrad vs discounted polgrad:
  * Define optimality criteria x for each (could be different for both, e.g., discounting-free refers to gain + bias converged, whilst discounted refers to v_gamma converged).
  * In the sampling scenario, we could reduce the term convergence here to "the norm of the gradient less than some tolerance number". So it doesn't need to end up living in the optimal region (as it might not work).
  * Find how many samples are needed for convergence
  * 

22. What's the origin of policy gradient methods?
Actually, there are two major ways for updating the policy gradients:
* Based on maximization of policy's value difference, e.g., gain difference --> This one leads to TRPO & PPO
* Based on gradient of the policy's value (this is basically the special case for the first scheme)


22. How's the proper way to implement discounted policy gradients?
* [Is the Policy Gradient a Gradient?](https://arxiv.org/abs/1906.07073)

23. What principles that enables conversion from DP setting to RL setting?
- Parameterizations --> enable generalization.

25. What are the examples of policy gradient success in practical scenarios?
Well, most of SOTA practical implementations are based on PPO, which is a variant of Policy Gradients.
 
26. What is the meaning of n >= 1-discount optimality criterion?
Some examples give a sign that n=1-discount optimality depicts a quicker process to reach the absorbing state with high rewards.

27. Any examples of gamma_blackwell that could be known in some environment?


28. Correlation between gradient descent and policy gradient?
[A Visual Tour From Gradient Descent to Policy Gradients](https://observablehq.com/@klezm/a-visual-tour-from-gradient-descent-to-policy-gradients)

29. Origin of Blackwell?

