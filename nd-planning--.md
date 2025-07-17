# Artificial Intelligence for Games: Study Notes
## Non-Deterministic Planning & Reinforcement Learning

---

## Table of Contents
1. [Non-Deterministic Planning](#1-non-deterministic-planning)
   - [Introduction to Non-Deterministic Problems](#11-introduction-to-non-deterministic-problems)
   - [Markov Reward Processes](#12-markov-reward-processes)
   - [Markov Decision Processes](#13-markov-decision-processes)
   - [Policy Evaluation](#14-policy-evaluation)
   - [Policy Optimization](#15-policy-optimization)
   - [Partially Observable MDPs](#16-partially-observable-mdps)
   - [Advanced Topics](#17-advanced-topics)
2. [Reinforcement Learning](#2-reinforcement-learning)
   - [Model-Free Methods](#21-model-free-methods)
   - [Policy Evaluation Methods](#22-policy-evaluation-methods)
   - [On-Policy Learning](#23-on-policy-learning)
   - [Off-Policy Learning](#24-off-policy-learning)

---

# 1. Non-Deterministic Planning

---
---
---
---
---

1. What real-world example is used to illustrate the extension from deterministic to non-deterministic planning problems?
2. Give three examples of uncertainties that could affect this problem.
3. When transitions become uncertain, what does the transition function become?
4. In non-deterministic planning, why can't we just find a single optimal path to the goal?
5. What is the key difference in what we need to find compared to deterministic planning?

---
---
---
---
---

## 1.1 Introduction to Non-Deterministic Problems

### The Goat, Wolf, Cabbage Problem Extension
Traditional planning finds optimal solutions for deterministic problems. However, real-world scenarios often involve uncertainty:

**Example uncertainties:**
- The goat might not like the cabbage
- The wolf might not be hungry
- The boat could sink on any trip

### Key Concept: Stochastic Transitions
When transitions become uncertain:
- Transition function becomes stochastic: **T: P(s'|s,a)** for all s,s' ∈ S, a ∈ A
- Rewards/costs become stochastic
- No guaranteed path to goal exists
- **Need a policy for every possible state**, not just states on a single path

---
---
---
---
---

1. What are the four components of a Markov Reward Process tuple?
2. How is the state transition probability matrix P defined mathematically?
3. What does the reward function R_s represent?
4. What is the range of the discount factor γ?
5. What key assumption does an MRP make about actions in each state?

---
---
---
---
---

## 1.2 Markov Reward Processes (MRP)

### Definition
A Markov Reward Process is a tuple **⟨S, P, R, γ⟩** where:
- **S**: Finite set of states
- **P**: State transition probability matrix
  - P_{s,s'} = Pr[S_{t+1} = s' | S_t = s]
- **R**: Reward function
  - R_s = E[R_t | S_t = s]
- **γ**: Discount factor, γ ∈ [0,1]

**Key insight**: MRP assumes a fixed action for each state (no choice of actions).

---
---
---
---
---

1. What additional component does an MDP have compared to an MRP?
2. In the Frozen Lake example, what are the rewards for reaching states (0,3) and (1,3)?
3. What are the transition probabilities for moving in the Frozen Lake grid world?
4. For finite horizon MDPs, why is γ = 1 allowed and what happens to policies?
5. List four reasons why discounting is used in infinite horizon MDPs.

---
---
---
---
---

## 1.3 Markov Decision Processes (MDP)

### Definition
A Markov Decision Process extends MRP by adding actions: **⟨S, A, P, R, γ⟩**
- **S**: Finite set of states
- **A**: Finite set of actions
- **P**: State transition probability matrix
  - P_{s,s'}^a = Pr[S_{t+1} = s' | S_t = s, A_t = a]
- **R**: Reward function
  - R_s = E[R_t | S_t = s]
- **γ**: Discount factor, γ ∈ [0,1]

### Example: Frozen Lake Grid World
```
States and Rewards:
+-------+-------+-------+-------+
| (0,0) | (0,1) | (0,2) | (0,3) |
|       |       |       |  +1   |
+-------+-------+-------+-------+
| (1,0) | HOLE  | (1,2) | (1,3) |
|       |       |       |  -1   |
+-------+-------+-------+-------+
| (2,0) | (2,1) | (2,2) | (2,3) |
|       |       |       |       |
+-------+-------+-------+-------+

Actions: Up, Down, Left, Right
Transition probabilities:
- 80% move in intended direction
- 10% slip left
- 10% slip right
```

### Finite vs Infinite Horizon MDPs

**Finite Horizon:**
- Process terminates after n steps
- γ = 1 allowed (all future rewards weighted equally)
- Policies become non-stationary: π(s) → π^t(s)

**Infinite Horizon:**
- Process continues forever or until terminal state
- γ < 1 usually required to ensure convergence
- Reasons for discounting:
  - Future rewards weighted less
  - Ensures convergence for infinite walks
  - Models human preference for immediate rewards
  - Financial applications (interest rates)

---
---
---
---
---

1. What does the value function v^π(s) represent mathematically?
2. Write the recursive Bellman equation for a given policy π.
3. How does the Bellman Optimality Equation differ from the standard Bellman Equation?
4. What are the two main methods for policy evaluation and their computational complexities?
5. In iterative Bellman updates, what is the update formula for v_{i+1}(s)?

---
---
---
---
---

## 1.4 Policy Evaluation

### Value Function
The value function v^π(s) represents the expected reward when following policy π from state s:

**v^π(s) = E[∑_{t=0}^∞ γ^t R(s_t) | s_0 = s, π]**

### Bellman Equation
The value function satisfies the recursive Bellman equation:

**v^π(s) = R(s) + γ ∑_{s'∈S} P(s'|s,π) v^π(s')**

### Bellman Optimality Equation
For the optimal policy π*:

**v^{π*}(s) = R(s) + γ max_{a∈A} ∑_{s'∈S} P(s'|s,a) v^{π*}(s')**

### Policy Evaluation Methods

1. **Direct Solution**: Solve system of linear equations (O(n³) complexity)
2. **Iterative Bellman Updates**: 
   ```
   v_{i+1}(s) ← R(s) + γ ∑_s' P(s'|s,π_i) v_i(s')
   ```

---
---
---
---
---

1. What are the two steps that Policy Iteration alternates between?
2. In the Policy Improvement step, how is the new policy π(s) determined?
3. What is the key difference between Policy Iteration and Value Iteration?
4. What is the stopping criterion for Value Iteration in terms of δ and ε?
5. What is the per-iteration complexity of Value Iteration for m actions and n states?

---
---
---
---
---

## 1.5 Policy Optimization

### Policy Iteration
Alternates between two steps:
1. **Policy Evaluation**: Given π, calculate v^π(s)
2. **Policy Improvement**: π(s) = argmax_{a∈A} ∑_{s'} P(s'|s,a) v*(s')

```python
repeat:
    v ← PolicyEvaluation(π, v, mdp)
    unchanged? ← true
    for each state s in S:
        if max_a ∑_s' P(s'|s,a)v[s'] > ∑_s' P(s'|s,π)v[s']:
            π[s] ← argmax_a ∑_s' P(s'|s,a)v[s']
            unchanged? ← false
until unchanged?
return π
```

### Value Iteration
Combines evaluation and improvement in one step:

```python
repeat:
    v ← v'
    δ ← 0
    for each state s in S:
        v'[s] ← R(s) + γ max_a ∑_s' P(s'|s,a) v[s']
        if |v'[s] - v[s]| > δ then δ ← |v'[s] - v[s]|
until δ < ε(1-γ)/γ
return v
```

**Complexity**: O(mn²) per iteration for m actions and n states

### Asynchronous Dynamic Programming

**Benefits over synchronous updates:**
- Some states may never be visited under optimal policy
- Different states converge at different rates
- Can prioritize important states

**Methods:**
1. **In-place Dynamic Programming**: Update v directly without copying
2. **Prioritized Sweeping**: Update states with largest Bellman error first
3. **Real-time Dynamic Programming (RTDP)**: 
   - Sample episodes and update along visited states
   - Initialize with lower bound
   - Follow greedy policy
   - Converges to optimal for stochastic shortest path problems

---
---
---
---
---

1. What two additional components does a POMDP have compared to an MDP?
2. What does the observation function Z_{s',o}^a represent?
3. What is a belief state and why is it necessary in POMDPs?
4. How can a POMDP be reformulated as an MDP, and what is the key challenge with this reformulation?
5. In the simple POMDP example, what is the accuracy of observations and the success rate of actions?

---
---
---
---
---

## 1.6 Partially Observable MDPs (POMDPs)

### Definition
POMDP adds partial observability: **⟨S, A, O, P, R, Z, γ⟩**
- **O**: Finite set of observations
- **Z**: Observation function
  - Z_{s',o}^a = P[O_{t+1} = o | S_t = s', A_t = a]

### Belief States
Since true state is hidden, maintain probability distribution over states:

**b(h) = (P(S_t = s¹|H_t = h_t), ..., P(S_t = s^n|H_t = h_t))**

Belief update after action a and observation o:
**b'(s_i) = Z_{s_i,o}^a ∑_{s_j∈S} P_{s_j,s_i}^a b(s_j)**

### POMDP as Belief MDP
Can reformulate POMDP as MDP over belief states:
- States: Distributions B over S (continuous!)
- Transitions: P(b'|a,b) = ∑_{o∈O} P(b'|o,a,b)P(o|a,b)
- Rewards: ρ(b) = ∑_{s∈S} R_s b(s)

### Simple POMDP Example
Two-state MDP with noisy observations:
- States: S = {s₁, s₂}, R(s₁) = 0, R(s₂) = 1
- Actions: STAY or GO (90% success rate)
- Observations: O = S with Z_{s',s'} = 0.6 (60% correct observation)

### QMDP Approximation
Assumes full observability after next step:
- Solve MDP under full observability to get Q(s,a)
- Policy: π_{QMDP}(b) = argmax_{a∈A} b·α_a
- Upper bounds optimal policy
- Struggles with information-gathering actions

---
---
---
---
---

1. In Factored MDPs, how are states represented differently from standard MDPs?
2. What is the dependency set D(x_i) for each factor x_i?
3. How does factoring reduce the complexity of transitions in the 10×10 grid example?
4. What is the basic approach of replanning for handling uncertainty?
5. What type of problems does replanning work well for?

---
---
---
---
---

## 1.7 Advanced Topics

### Factored MDPs
States represented as combinations of factors X = {x₁, x₂, ..., x_d}:
- Each factor x_i has dependency set D(x_i) ⊆ X
- Transitions: p(x_i'|X,a) = p(x_i'|D(x_i),a)
- Reduces transition complexity from |S|×|A|×|S| to |X|×|A|×|D(x)|

**Example**: 10×10 grid with 4 actions
- Standard: 400 transitions
- Factored: 2×4×1 = 8 transitions (+1/-1 for x,y)

### Replanning
Approximate by assuming most likely future:
1. Compute most likely path using A* search
2. Execute first action
3. If unexpected state reached, replan
4. Works well for "probabilistic uninteresting" problems

---

# 2. Reinforcement Learning

---
---
---
---
---

1. List three reasons why model-free methods are needed instead of MDPs.
2. What is an episode in reinforcement learning notation?
3. Define exploration and exploitation in the context of reinforcement learning.
4. What do the samples (s,a,s') and r,s represent in terms of the underlying distributions?
5. Why might MDPs be impractical for real-world problems?

---
---
---
---
---

## 2.1 Model-Free Methods

### Motivation
MDPs require complete model knowledge, but often:
- Transition functions unknown
- State spaces too large
- Partial observability
- Continuous spaces

### Key Concepts
**Episodes**: e = s₁,a₁,r₁,s₂,a₂,r₂,...,s_l,a_l,r_l,s_{l+1}
- (s,a,s') samples from P(s'|s,a)
- r,s samples from R(s)

**Exploration vs Exploitation**:
- **Exploitation**: Use current policy estimate
- **Exploration**: Try unknown state-action pairs

---
---
---
---
---

1. How does Monte Carlo policy evaluation estimate the value function?
2. What is G_t(x) in the Monte Carlo method?
3. What is the incremental update formula for Monte Carlo learning?
4. List four key properties of Monte Carlo methods.
5. What is the TD target and TD error in Temporal Difference learning?

---
---
---
---
---

## 2.2 Policy Evaluation Methods

### Monte Carlo (MC) Policy Evaluation
Estimates value by averaging complete episode returns:

**v^π(s) = ∑_{x∈X(s)} G_t(x) / |X(s)|**

where G_t(x) = ∑_{i=t}^l γ^i R(x_i) is the return from time t

**Incremental update**:
```
v(s_t) ← v(s_t) + α[G_t(x) - v(s_t)]
```

**Properties**:
- Unbiased estimate
- High variance
- Requires complete episodes
- Simple to implement

### Temporal Difference (TD) Learning
Updates value estimates after each step:

**v(s_t) ← v(s_t) + α[R(s_{t+1}) + γv(s_{t+1}) - v(s_t)]**

- **TD target**: R(s_{t+1}) + γv(s_{t+1})
- **TD error**: R(s_{t+1}) + γv(s_{t+1}) - v(s_t)

**Properties**:
- Biased estimate (uses current v estimate)
- Low variance
- Online learning
- Works with incomplete episodes

### MC vs TD Comparison

| Monte Carlo | Temporal Difference |
|-------------|-------------------|
| High variance, zero bias | Low variance, some bias |
| Must wait for episode end | Online, step-by-step |
| Converges to v^π(s) | Converges to v^π(s) (tables) |
| Not sensitive to initial value | Sensitive to initial value |
| Exploits all episode info | Exploits Markov property |

---
---
---
---
---

1. What does SARSA stand for and why is it called that?
2. Write the SARSA Q-value update equation.
3. How does ε-greedy exploration determine action probabilities?
4. What are the two conditions for GLIE convergence to the optimal policy?
5. How is GLIE typically implemented in practice with respect to ε?

---
---
---
---
---

## 2.3 On-Policy Learning

### SARSA (State-Action-Reward-State-Action)
On-policy TD control using Q-values:

**Q(S,A) ← Q(S,A) + α[R + γQ(S',A') - Q(S,A)]**

```python
init Q(s,a) for all s ∈ S, a ∈ A
for n episodes:
    init s
    choose a from A(s) based on Q (e.g. ε-greedy)
    repeat until episode finished:
        s',r = query_Env(s,a)
        choose a' from A(s') based on Q
        Q(s,a) ← Q(s,a) + α(r + γQ(s',a) - Q(s,a))
        s ← s', a ← a'
    until s is terminal
```

### ε-Greedy Exploration
Balances exploration and exploitation:

```
π(a|s) = {
    ε/m + (1-ε)  if a = argmax_a Q(s,a)
    ε/m          otherwise
}
```
where m = |A(s)|

### GLIE (Greedy in the Limit with Infinite Exploration)
Conditions for convergence to optimal policy:
1. All state-action pairs explored infinitely: lim_{k→∞} N_k(s,a) = ∞
2. Policy converges to greedy: lim_{k→∞} π_k(a|s) = 1(a = argmax_{a'} q_k(s,a'))

**Implementation**: ε_k = 1/k

---
---
---
---
---

1. Give three motivations for using off-policy learning methods.
2. What is the importance sampling formula for correcting different distributions?
3. Write the Q-Learning update equation and explain how it differs from SARSA.
4. In Q-Learning, what is the behavior policy η used for versus the target policy π?
5. What are the key differences between SARSA and Q-Learning in terms of policy learning?

---
---
---
---
---

## 2.4 Off-Policy Learning

### Motivation
Learn optimal policy while following exploratory behavior:
- Learn from other agents/humans
- Reuse old experience
- Learn multiple policies simultaneously

### Importance Sampling
Corrects for different state distributions:

**E_{X~P}[f(X)] = E_{X~Q}[P(X)/Q(X) · f(X)]**

For TD with behavior policy η and target policy π:
**v(S_t) ← v(S_t) + α · π(A_t|S_t)/η(A_t|S_t) · [r_{t+1} + γv(S_{t+1}) - v(S_t)]**

### Q-Learning
Off-policy TD control without importance sampling:

**Q(S_t,A_t) ← Q(S_t,A_t) + α[r_{t+1} + γ max_a Q(S_{t+1},a) - Q(S_t,A_t)]**

```python
init Q(s,a) for all s ∈ S, a ∈ A
for n episodes:
    init s
    repeat until episode finished:
        choose a from A(s) with behavior policy η
        s',r = query_Env(s,a)
        Q(s,a) ← Q(s,a) + α(r + γ max_a Q(s',a) - Q(s,a))
        s ← s'
    until s is terminal
```

**Key insight**: 
- Behavior policy η explores (e.g., ε-greedy)
- Target policy π is greedy: π(s) = argmax_a Q(s,a)
- Converges to Q*(s,a) under mild conditions

### SARSA vs Q-Learning
- **SARSA**: On-policy, learns Q^π for current policy
- **Q-Learning**: Off-policy, learns Q* regardless of behavior
- SARSA is more conservative (considers exploration in value estimates)
- Q-Learning may overestimate in stochastic environments

---

## Summary

### Non-Deterministic Planning
- MDPs model sequential decision problems with uncertainty
- Value/Policy iteration solve MDPs when model is known
- POMDPs handle partial observability via belief states
- Various approximations handle large state spaces

### Reinforcement Learning
- Learn optimal behavior from experience without model
- MC methods use complete returns (high variance, unbiased)
- TD methods bootstrap from value estimates (low variance, biased)
- On-policy (SARSA) vs Off-policy (Q-Learning) trade exploration handling

### Key Algorithms Complexity
- Value/Policy Iteration: O(mn²) per iteration
- SARSA/Q-Learning: O(1) per step update
- POMDP exact solution: PSPACE-complete

### When to Use What
- **Known model, small state space**: Value/Policy Iteration
- **Unknown model, need optimal policy**: Q-Learning
- **Unknown model, safer exploration**: SARSA
- **Partial observability**: POMDP methods or state aggregation
- **Large/continuous spaces**: Function approximation (neural networks)