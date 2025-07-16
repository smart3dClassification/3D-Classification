## Table of Contents
1. [Agents](#1-agents)
2. [Deterministic Planning](#2-deterministic-planning)

---

# 1. Agents

### Core Concepts
- **AI Definition**: Making computers act rationally or human-like
- **G Definition**: An interactive experience
- **Key Insight**: For games, AI is primarily about the actions of Agents

### Example Challenges
1. What is the best action in a particular state to achieve a goal?
2. How do human players act?
3. Does the player behavior belong to a human or an AI?
4. How does the environment influence player behavior?

---
---
---
---
---

1. Explain the difference between making computers act rationally versus making them act human-like in the context of game AI.
2. Why are agent actions considered the primary focus of AI in games?
3. Design a scenario where determining whether behavior belongs to a human or AI would be particularly challenging.
4. How might the environment influence player behavior differently in a horror game versus a puzzle game?

---
---
---
---
---

## 1.2 General Agent-Environment Framework

### Components
```
Environment ←→ Agent
     ↓           ↓
Game State    Player View
     ↓           ↓
   GAME    →  action
```

The framework consists of:
- **Environment**: Contains the complete game state
- **Agent**: Receives observations and produces actions
- **Game State**: Complete representation of the game world
- **Player View**: What the agent observes (may be partial)
- **Action**: Agent's response to observations

---
---
---
---
---

1. Draw and explain your own example of the Agent-Environment framework for a specific game (e.g., Pac-Man or Tetris).
2. What is the key difference between Game State and Player View? Provide an example where these differ significantly.
3. How does the interaction flow between the Environment and Agent components work in a turn-based game versus a real-time game?
4. Design a simple game scenario and identify all five components of the framework.

---
---
---
---
---

## 1.3 Formal Mathematical Setting

### Markov Process Foundation
A game can be described as a discrete time, homogeneous Markov process consisting of:
- **Set of states**: S
- **Transition function**: t(s) T: P(s'|s) for all s,s' ∈ S

### Agent Integration
When considering an agent:
- At each step t, the agent can choose an action: **a ∈ A(s_t)**
  - Where A(s_t) is the set of all valid actions in state s_t
- New transition function: **t(s,a)** for all s,s' ∈ S and all a ∈ A(s_t)

### Episode and Behavior
- **Episode**: s₁, a₁, s₂, a₂, ..., s_t, a_t, s_{t+1}
- **Behavior**: a₁, a₂, ..., a_t

---
---
---
---
---

1. Explain what makes a process "Markov" and why this property is important for game AI.
2. Given a simple grid world where an agent can move in 4 directions, define S, A(s), and provide examples of invalid actions for certain states.
3. What is the difference between an episode and behavior? Why might we want to track both?
4. How does the transition function change when we introduce agent actions compared to a pure Markov process?
5. Create an example episode of length 5 for a simple game and extract the corresponding behavior.

---
---
---
---
---

## 1.4 Policies and Agents

### Policy Definition
A policy π is a mapping between states S and actions A:
- **Deterministic policy**: π(s) = a, where s ∈ S and a ∈ A(s)
- **Stochastic policy**: π(s,a) = P(a|s)
  - π(s,a) is the likelihood of taking action a in state s

### Policy Dependencies
Agent policies depend on:
1. The environment they are acting in
2. The purpose of the agent
3. The type of policy function used

---
---
---
---
---

1. When would you prefer a stochastic policy over a deterministic policy? Provide a specific game example.
2. How would the policy for a chess AI differ if its purpose was to teach beginners versus defeating grandmasters?
3. Design both a deterministic and stochastic policy for a simple rock-paper-scissors agent and explain the advantages of each.
4. Explain how changing the environment (e.g., adding fog of war) might require changes to an agent's policy.

---
---
---
---
---

## 1.5 Environment Properties

### Observability
1. **Fully Observable Setting**:
   - Agent gets information of the complete Game State
   - The game state is usually Markov
   - Example: Go (all pieces visible on board)

2. **Partially Observable Setting**:
   - Agent sees only parts of the game state
   - Not all relevant information may be known
   - Decisions might run on an abstraction of the true game state
   - Example: StarCraft II (fog of war, opponent's resources hidden)

### Determinism
1. **Deterministic Transitions**: t(s,a) → s'
   - Same action in same state always has same result
   - Examples: Sliding puzzles, riddles

2. **Non-Deterministic Transitions**: T: P(s'|s,a)
   - Same action in same state might lead to different results
   - Most games contain non-determinism to maintain challenge
   - Examples: Chess (opponent moves), Poker (cards), Hearthstone (RNG)

### Additional Properties
- **Static vs. Dynamic Environments**:
  - Static: Environment only changes through player actions
  - Dynamic: Environment changes without player action (real-time games)

- **Finite vs. Infinite Horizon**:
  - Finite: Process ends after certain number of steps
  - Infinite: Continues until goal state reached or indefinitely

- **Discrete vs. Continuous**: State and action spaces
- **Number of Agents**: Single or multi-agent
- **Agent Attitudes**: Collaborative or antagonistic
- **Model Knowledge**: Known rules vs. unknown rules

---
---
---
---
---

1. How does partial observability affect the Markov property of a game state? Explain with an example.
2. Why is chess listed as an example of non-deterministic transitions when the rules themselves are deterministic?
3. Design a game that is fully observable but non-deterministic, and explain your design choices.
4. How would an AI agent's strategy differ in a static versus dynamic environment? Use specific examples.
5. What challenges arise when transitioning from discrete to continuous state/action spaces?
6. Describe a scenario where agents must switch between collaborative and antagonistic attitudes mid-game.

---
---
---
---
---

## 1.6 Agent Goals

### Types of Policy Goals
1. **Provide game design element**
   - Examples: Monsters, creeps, NPCs

2. **Act human-like**
   - Mimic average player (farming bots)
   - Master the game like a human (computer opponents)

3. **Maximize reward function**
   - Examples: Winning, collecting points

4. **Minimize cost function**
   - Examples: Minimize time, resources used

---
---
---
---
---

1. Why might a game designer want an AI that acts sub-optimally rather than one that maximizes rewards?
2. What are the key differences between mimicking an average player versus mastering a game like a human?
3. Design a reward function and a cost function for the same game scenario. How do they differ in implementation?
4. How would you balance multiple goals (e.g., act human-like while providing appropriate challenge)?

---
---
---
---
---

## 1.7 Types of Policy Functions

### Reflex Agents
**Characteristics**:
- Provides set of rules tested in particular order
- Format: condition(s) → action
- Represent known policies but don't optimize
- Rules can be organized in hierarchy/tree
- Very efficient for appropriate state/action spaces
- Most PvE games use reflex agents

**Example: "Autocamp 2000" Bot**
```
Rules:
1. If invited by group → join group
2. If in group → follow leader
3. If sees monster → attack
4. If message ends with "?" → respond "Dude?"
5. If message ends with "!" → respond "Dude!"
6. If message ends with "." → respond randomly: "Okie", "Sure", or "Right on"
7. EXCEPTION: If addressed by name → respond "Lag."
```

---
---
---
---
---

1. What are the main limitations of reflex agents, and in what scenarios do they break down?
2. Design a reflex agent for a simple tower defense enemy. What rules would you prioritize and why?
3. How does rule ordering affect the behavior of the "Autocamp 2000" bot? What would happen if rules were reordered?
4. Why are reflex agents particularly suitable for PvE games but less so for PvP scenarios?

---
---
---
---
---

### Model-Based Reflex Agents
**Key Features**:
- Maintain internal state from observation sequence
- Necessary for partially observable settings
- Rules applied based on internal state

**Example**: Agent pursuing moving object
- If object visible → move to object
- If object invisible → go to last known position

---
---
---
---
---

1. How does maintaining internal state help overcome the limitations of simple reflex agents?
2. Design a model-based reflex agent for a stealth game guard. What internal state would it need to track?
3. What happens when the internal state becomes outdated or incorrect? How can this be mitigated?
4. Compare the memory requirements of model-based reflex agents versus simple reflex agents.

---
---
---
---
---

### Goal-Based Agents
**Characteristics**:
- Require reward/cost function to optimize
- Simple immediate optimization: Take highest reward action
- Sequential planning: Find action sequence maximizing total reward
- Planning ahead is crucial for intelligence

**Example**: Invest today to earn tomorrow

---
---
---
---
---

1. Why is planning ahead considered crucial for intelligence in goal-based agents?
2. Design a scenario where immediate optimization leads to poor long-term outcomes.
3. How would you implement a goal-based agent for a resource management game?
4. What computational challenges arise when planning many steps ahead?

---
---
---
---
---

### Utility-Based Agents (Markov Decision Processes)
**Key Concepts**:
- Handle non-deterministic settings where future rewards are uncertain
- Utility/value functions describe cumulated expected reward
- Depend on γ parameter (discount factor):
  - γ = 1: All rewards count equally
  - γ < 1: Immediate rewards preferred
- Greedily choose action maximizing utility

**Requirements**:
- Known transition function
- Reasonably small state/action spaces

---
---
---
---
---

1. Explain why different values of γ lead to different agent behaviors. When would you use γ = 0.5 versus γ = 0.9?
2. How do utility-based agents differ from simple goal-based agents in handling uncertainty?
3. What happens when the state/action space becomes too large for utility-based approaches?
4. Design a utility function for a trading card game AI that must balance winning and entertainment value.

---
---
---
---
---

### Learning Agents (Reinforcement Learning)
**Features**:
- Approximate utility function from observed episodes
- Sometimes learn policy directly without utility approximation
- Can abstract and transfer policies between similar settings
- Handle unknown transition functions
- Work with large state/action spaces

---
---
---
---
---

1. What advantages do learning agents have over utility-based agents that require known transition functions?
2. How might a learning agent transfer knowledge from one game level to another similar level?
3. What are the risks of deploying a learning agent that continues to learn during gameplay?
4. Compare the sample efficiency of learning a policy directly versus learning a utility function first.

---
---
---
---
---

## 1.8 Termination and Proper Policies

### Termination Guarantees
- **Finite horizon**: Termination guaranteed
- **Infinite horizon**: Not all policies terminate

### Proper Policy
Definition: A policy that reaches a terminal state

**For Cost-Based Settings**:
- Optimal policies must be proper
- Only proper policies have finite cost

**For Reward-Based Settings**:
- γ = 1: Σ(γⁱrᵢ) → ∞, policies might not terminate
- γ < 1: Σ(γⁱrᵢ) → C, can force termination with reward > C at terminal states

---
---
---
---
---

1. Why must optimal policies be proper in cost-based settings? What happens if they aren't?
2. Design a game scenario where an improper policy might seem attractive but ultimately fails.
3. How does the discount factor γ help ensure termination in reward-based settings?
4. What techniques can force termination in infinite horizon games without explicitly defined terminal states?

---
---
---
---
---

# 2. Deterministic Planning

## 2.1 Introduction to Deterministic Planning

### When to Use Deterministic Planning
- Few purely deterministic games are interesting
- Useful for solving AI subtasks:
  - Finding exits in buildings
  - Routing to targets
- Approximating non-deterministic problems
  - Example: Assume opponent uses identical policy in Go/Chess

---
---
---
---
---

1. Why are purely deterministic games often considered less interesting than non-deterministic ones?
2. How can deterministic planning be used effectively within a larger non-deterministic game?
3. What are the risks of approximating non-deterministic problems as deterministic ones?
4. Describe three AI subtasks in modern games where deterministic planning would be appropriate.

---
---
---
---
---

## 2.2 Formal Framework

### Assumptions
Static and fully-observable environments

### Components
- **States**: S = {s₁, ..., sₙ}
- **Actions**: A(s) for each state s ∈ S
- **Reward function**: R: R(s) (negative = cost function)
- **Transition function**: T: S×A → S: t(s,a) = s'
- **Goal**: Maximize cumulated rewards (minimize cost)

### Cumulated Reward/Cost
**Formula**: Σᵢ₌₁ˡ γⁱrᵢ with 0 < γ ≤ 1
- γ = 1: All rewards count equally
- γ < 1: Early rewards count more

### Episode Structure
s₁, a₁, r₁, s₂, a₂, r₂, s₃, a₃, r₃, ..., sₗ, aₗ, rₗ, sₗ₊₁

### Termination Conditions
1. Reaching terminal state (infinite horizon)
2. Process ends after k steps (finite horizon)

---
---
---
---
---

1. Why are static and fully-observable assumptions necessary for deterministic planning?
2. How would you convert a reward maximization problem into a cost minimization problem?
3. Calculate the cumulated reward for an episode with rewards [10, 5, 3, 1] using γ = 0.8.
4. What are the trade-offs between finite and infinite horizon formulations?
5. Design a simple deterministic planning problem and specify all components of the formal framework.

---
---
---
---
---

## 2.3 Search Trees and Graphs

### Graph Structure
- **Nodes**: States S
- **Edges**: Transitions t(s,a)
- **Weights**: Rewards/costs on nodes or edges

### Search Tree Properties
- All possible episodes from start state form search tree
- Nodes may appear multiple times with different cumulated costs
- Tree may be infinite even for finite graphs:
  - Finite horizon: All paths same length
  - Infinite horizon with terminals: Infinite paths possible

### Example: Gridworld
```
Grid Layout:
[start] [ ] [ ] [goal]
[  ]    [X] [ ] [  ]
[  ]    [ ] [X] [  ]
[  ]    [ ] [ ] [  ]
```
- States: Grid positions (x,y)
- Actions: Up, Down, Left, Right
- Obstacles: Block certain positions
- Cost: 1 per action OR reward at goal

---
---
---
---
---

1. Explain why the same state can appear multiple times in a search tree with different costs.
2. How can a finite state graph lead to an infinite search tree?
3. For the gridworld example, calculate the optimal path length and draw a portion of the search tree.
4. What is the relationship between cycles in the graph and the size of the search tree?
5. How would you modify the gridworld to prevent infinite loops while maintaining interesting gameplay?

---
---
---
---
---

## 2.4 Search Algorithms

### Breadth-First Search (BFS)

**Characteristics**:
- Explores paths by length (number of steps)
- Finds shortest path in steps (not necessarily minimal cost)
- Memory: O(m) where m = number of unterminated paths
- m grows exponentially: b → b² → b³ → ... → bᵈ
- Complete: Will find optimal solution eventually

**Pseudocode**:
```python
FUNCTION path BFS(State start, State[] terminals)
    Q = List()
    Q.insert(new path(start))
    WHILE(Q.notIsEmpty())
        path aktTraj = Q.getFirst()
        IF aktTraj.last() in terminals THEN
            return aktTraj
        ELSE
            FOR State n in aktTraj.last().transitions() DO
                path newTraj = aktTraj.extend(n)
                Q.addLast(newTraj)
            ENDDO
        ENDIF
    ENDWHILE
    RETURN NULL
ENDFUNCTION
```

---
---
---
---
---

1. Why does BFS find the shortest path in steps but not necessarily the minimal cost path?
2. Calculate the memory requirements for BFS on a tree with branching factor 3 at depth 5.
3. Modify the BFS pseudocode to track visited states and prevent revisiting them.
4. In what scenarios would BFS be preferred over other search algorithms?
5. Explain why the memory usage grows exponentially with depth.

---
---
---
---
---

### Uniform Cost Search (Dijkstra's Algorithm)

**Key Features**:
- Extends minimal cost path each step
- Uses priority queue (ascending cost order)
- Requires monotonically increasing costs
- Guarantees minimal cost for first terminal path found

**Pseudocode**:
```python
FUNCTION path uniformCostSearch(State start, State[] terminals)
    Q = new PriorityQueue(ascending)
    Q.insert(new path(start), 0)
    WHILE(Q.notIsEmpty())
        path aktTraj = Q.getFirst()
        IF aktTraj.last() in terminals THEN
            return aktTraj
        ELSE
            FOR State n in aktTraj.last().transitions() DO
                path newTraj = aktTraj.extend(n)
                Q.insert(newTraj, newTraj.cost)
            ENDDO
        ENDIF
    ENDWHILE
    RETURN NULL
ENDFUNCTION
```

---
---
---
---
---

1. Why does Uniform Cost Search require monotonically increasing costs?
2. How does the priority queue ensure we find the optimal solution?
3. Compare the time and space complexity of Uniform Cost Search with BFS.
4. Design a scenario where Uniform Cost Search significantly outperforms BFS.
5. What modifications would be needed to handle negative edge costs?

---
---
---
---
---

### Depth-First Search (DFS)

**Characteristics**:
- Extends one path until termination
- Memory: O(d·b) where d = current path length
- Cannot guarantee termination in infinite horizon settings

**Pseudocode**:
```python
FUNCTION path DFS(State start, State[] terminals)
    Q = List()
    Q.insert(new path(start))
    WHILE(Q.notIsEmpty())
        path aktTraj = Q.getFirst()
        IF aktTraj.last() in terminals THEN
            return aktTraj
        ELSE
            FOR State n in aktTraj.last().transitions() DO
                path newTraj = aktTraj.extend(n)
                Q.addFirst(newTraj)  # Note: addFirst for DFS
            ENDDO
        ENDIF
    ENDWHILE
    RETURN NULL
ENDFUNCTION
```

---
---
---
---
---

1. Why does DFS use O(d·b) memory instead of the exponential memory required by BFS?
2. What causes DFS to potentially not terminate in infinite horizon settings?
3. When would DFS find the optimal solution by chance?
4. How does changing addLast to addFirst convert BFS to DFS?
5. Design a graph where DFS performs much worse than BFS in finding the goal.

---
---
---
---
---

### Depth-Limited Search & Iterative Deepening

**Depth-Limited Search (DLS)**:
- Terminates paths at limit l
- Finds optimal if path length ≤ l
- May find suboptimal or no solution if optimal > l

**Iterative Deepening**:
- Run multiple DLS with increasing l
- Guarantees finding solution
- Approximately doubles search time

---
---
---
---
---

1. How do you choose an appropriate depth limit for DLS without knowing the solution depth?
2. Why does iterative deepening only approximately double the search time despite repeated searches?
3. Calculate the total nodes explored by iterative deepening for a tree with branching factor 2 and solution at depth 4.
4. When would iterative deepening be preferred over standard BFS?
5. How can you modify iterative deepening to work with non-uniform costs?

---
---
---
---
---

## 2.5 Informed Search: A* Algorithm

### Core Concepts
**Heuristic Function**: h(s,s') estimates optimal cost from s to s'
- **Optimistic/Lower-bounding**: h(s,s') ≤ opt(s,s')
- **Lower bound formula**: LB(p) = cost(p) + min_{s'∈Terminal} h(s,s')

### A* Properties
- Requires monotonically increasing costs
- If solution p' found with cost(p') < LB(p), no need to extend p
- Guarantees optimal solution

**Pseudocode**:
```python
FUNCTION path astarSearch(State start, State[] terminals)
    Q = new PriorityQueue(ascending)
    Q.insert(new path(start), 0)
    WHILE(Q.notIsEmpty())
        path aktTraj = Q.getFirst()
        IF aktTraj.last() in terminals THEN
            return aktTraj
        ELSE
            FOR State n in aktTraj.last().transitions() DO
                path newTraj = aktTraj.extend(n)
                f_value = newTraj.cost + h(newTraj.last(), terminals)
                Q.insert(newTraj, f_value)
            ENDDO
        ENDIF
    ENDWHILE
    RETURN NULL
ENDFUNCTION
```

### Example: Shortest Path in Road Network
- **States**: Road crossings
- **Actions**: Taking outgoing streets
- **Cost**: Distance of road segment
- **Heuristic**: Euclidean distance (straight line)
- **Result**: Dramatically reduces search space

---
---
---
---
---

1. Why must the heuristic function be optimistic (lower-bounding) for A* to guarantee optimal solutions?
2. Design three different heuristic functions for the gridworld problem and compare their effectiveness.
3. What happens if the heuristic overestimates the actual cost? Provide a specific example.
4. How does the quality of the heuristic affect the number of nodes explored by A*?
5. Prove that Uniform Cost Search is a special case of A* with a specific heuristic.
6. Why is Euclidean distance a valid heuristic for road networks but not for maze problems?

---
---
---
---
---

## 2.6 Classic Example: Goat, Wolf, Cabbage Problem

### Problem Setup
- Transport goat, wolf, and cabbage across river
- Boat carries you + one item
- Constraints:
  - Wolf alone with goat → wolf eats goat
  - Goat alone with cabbage → goat eats cabbage

### State Representation
State = (boat, wolf, goat, cabbage) as binary (0=start side, 1=far side)
- Invalid states: w=g≠b, g=c≠b
- Start: (0,0,0,0)
- Goal: (1,1,1,1)

### Solution Approach
Find shortest path in state graph avoiding invalid states

---
---
---
---
---

1. Draw the complete state space for this problem, marking invalid states.
2. How many valid states exist in this problem? How many total states?
3. What makes this problem particularly suitable for state-space search?
4. Design a similar river-crossing puzzle with different constraints.
5. What heuristic could you use for A* search in this problem?

---
---
---
---
---

## 2.7 Visibility Graphs for Pathfinding

### Problem Setting
- 2D continuous space with polygon obstacles
- Find shortest path between two points

### Visibility Graph Construction

**Definition**: Graph G_U(V,E) where:
- **Nodes V**: All polygon corners
- **Edges E**: Connections between corners that:
  - Don't intersect polygon borders
  - Connect different polygons
  - Include convex hull of each polygon

**Complexity**: O(n²) optimal algorithm

### Handling Extended Objects

**Challenge**: Agents have spatial extent (not point objects)

**Solution Approach**:
1. Approximate agent as circle (rotation invariant)
2. Approximate circle as polygon (e.g., hexagon)
3. Expand obstacles by Minkowski sum
4. Build visibility graph on expanded obstacles

**Trade-offs**:
- Paths not optimal
- Conservative passage handling
- Angular curves
- Each agent shape needs separate graph

### Alternative Abstractions
1. **Polygon simplification**: Reduce corner count
2. **Hierarchical routing**: For long distances
3. **Pre-calculated paths**: Store common routes
4. **Grid-based graphs**: Overlay grid, route through centers
5. **Sampled graphs**: For open environments

---
---
---
---
---

1. Why are only polygon corners included as nodes in the visibility graph?
2. Explain the Minkowski sum approach for handling extended objects. What are its limitations?
3. Compare the trade-offs between visibility graphs and grid-based pathfinding.
4. How would you modify the visibility graph approach for 3D environments?
5. Design a hierarchical routing system for a large game world with cities and roads.
6. When would pre-calculated paths be advantageous, and what are their memory requirements?

---
---
---
---
---

## 2.8 Summary of Deterministic Planning

### Key Requirements
- Static, discrete, deterministic environment
- Known transition function
- Full observability

### Limitations & Extensions
- Continuous settings must be discretized
- Dynamic environments solvable if dynamics known
- Partial observability leads to non-determinism
- Can approximate non-deterministic problems

### Practical Applications
- Subproblem solving in complex games
- Approximation for non-deterministic scenarios
- Foundation for more advanced planning methods

---
---
---
---
---

1. How can deterministic planning techniques be adapted for slightly non-deterministic environments?
2. What are the main challenges in discretizing continuous environments for planning?
3. Describe how you would use deterministic planning as a component in a larger game AI system.
4. What types of game problems are poorly suited for deterministic planning approaches?
5. How do modern games typically handle the computational complexity of planning algorithms?

---
---
---
---
---