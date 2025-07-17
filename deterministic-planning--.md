## Table of Contents
1. [Agents](#1-agents)
2. [Deterministic Planning](#2-deterministic-planning)

---

# 1. Agents

---
---
---
---
---

What are the two definitions mentioned at the beginning, and what is the key insight about AI in games?
What are the four example challenges that agents face in game AI?

---
---
---
---
---

### Core Concepts
- **AI Definition**: Making computers act rationally or human-like
- **G Definition**: An interactive experience
- **Key Insight**: For games, AI is primarily about the actions of Agents

### Example Challenges
1. What is the best action in a particular state to achieve a goal?
2. How do human players act?
3. Does the player behavior belong to a human or an AI?
4. How does the environment influence player behavior?

## 1.2 General Agent-Environment Framework

---
---
---
---
---

What are the five main components of the agent-environment framework and how do they interact with each other?
What is the difference between the game state and player view?

---
---
---
---
---

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

## 1.3 Formal Mathematical Setting

---
---
---
---
---

What two components make up a discrete time, homogeneous Markov process?
How does the transition function change when an agent is integrated into the system?
What is an episode and what is a behavior in the context of agent actions?

---
---
---
---
---

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

## 1.4 Policies and Agents

---
---
---
---
---

What is the difference between a deterministic and stochastic policy?
What three factors do agent policies depend on?

---
---
---
---
---

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

## 1.5 Environment Properties

---
---
---
---
---

What distinguishes a fully observable setting from a partially observable setting?
What is the key difference between deterministic and non-deterministic transitions?
What are the eight additional properties that characterize environments beyond observability and determinism?

---
---
---
---
---

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

## 1.6 Agent Goals

---
---
---
---
---

What are the four different types of policy goals that agents can have?

---
---
---
---
---

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

## 1.7 Types of Policy Functions

---
---
---
---
---

What are the main characteristics of reflex agents and what format do their rules follow?
What are the seven rules of the "Autocamp 2000" Bot example and what is the exception rule?

---
---
---
---
---

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

How do model-based reflex agents differ from regular reflex agents?
What strategy does the example agent use when pursuing a moving object?

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

What do goal-based agents require to function and what are their two main optimization approaches?
Why is planning ahead considered crucial for intelligence?

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

What is the γ parameter in utility-based agents and how does its value affect decision making?
What are the two main requirements for utility-based agents to work effectively?

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

What do learning agents approximate and what are their key advantages over utility-based agents?

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

## 1.8 Termination and Proper Policies

---
---
---
---
---

What is the definition of a proper policy?
How do the requirements for proper policies differ between cost-based and reward-based settings?

---
---
---
---
---

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

# 2. Deterministic Planning

## 2.1 Introduction to Deterministic Planning

---
---
---
---
---

Why are few purely deterministic games interesting, and what are deterministic planning methods useful for instead?

---
---
---
---
---

### When to Use Deterministic Planning
- Few purely deterministic games are interesting
- Useful for solving AI subtasks:
  - Finding exits in buildings
  - Routing to targets
- Approximating non-deterministic problems
  - Example: Assume opponent uses identical policy in Go/Chess

## 2.2 Formal Framework

---
---
---
---
---

What assumptions are made about the environment in deterministic planning?
What is the formula for cumulated reward/cost and how does the γ parameter affect it?
What are the two possible termination conditions for an episode?

---
---
---
---
---

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

## 2.3 Search Trees and Graphs

---
---
---
---
---

How do the nodes, edges, and weights correspond to the planning problem components in a graph structure?
Why might nodes appear multiple times in a search tree and when can a tree be infinite even for finite graphs?
What are the states, actions, and costs in the Gridworld example?

---
---
---
---
---

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

## 2.4 Search Algorithms

---
---
---
---
---

What does BFS explore paths by and what is its memory complexity?
Why does BFS find the shortest path in steps but not necessarily the minimal cost path?

---
---
---
---
---

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

What type of data structure does Uniform Cost Search use and what ordering?
What requirement must costs satisfy for Uniform Cost Search to guarantee finding the minimal cost path?

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

What is the memory complexity of DFS and why can't it guarantee termination in certain settings?
What is the key difference in the pseudocode between DFS and BFS?

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

What is the main limitation of Depth-Limited Search and how does Iterative Deepening address it?
What is the trade-off when using Iterative Deepening?

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

## 2.5 Informed Search: A* Algorithm

---
---
---
---
---

What property must a heuristic function have to be considered optimistic/lower-bounding?
What is the lower bound formula for a path and when can we stop extending a path in A*?

---
---
---
---
---

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

---
---
---
---
---

In the road network example, what heuristic is used and why is it admissible?

---
---
---
---
---

### Example: Shortest Path in Road Network
- **States**: Road crossings
- **Actions**: Taking outgoing streets
- **Cost**: Distance of road segment
- **Heuristic**: Euclidean distance (straight line)
- **Result**: Dramatically reduces search space

## 2.6 Classic Example: Goat, Wolf, Cabbage Problem

---
---
---
---
---

What are the constraints in the goat, wolf, cabbage problem?
How is the state represented and what makes certain states invalid?

---
---
---
---
---

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

## 2.7 Visibility Graphs for Pathfinding

---
---
---
---
---

What defines the nodes and edges in a visibility graph?
What is the optimal complexity for constructing a visibility graph?

---
---
---
---
---

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

---
---
---
---
---

How is the challenge of agents having spatial extent addressed in visibility graphs?
What are the four trade-offs of using this approach?

---
---
---
---
---

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

---
---
---
---
---

What are five alternative abstractions to visibility graphs for pathfinding?

---
---
---
---
---

### Alternative Abstractions
1. **Polygon simplification**: Reduce corner count
2. **Hierarchical routing**: For long distances
3. **Pre-calculated paths**: Store common routes
4. **Grid-based graphs**: Overlay grid, route through centers
5. **Sampled graphs**: For open environments

## 2.8 Summary of Deterministic Planning

---
---
---
---
---

What are the three key requirements for deterministic planning?
How can deterministic planning be applied to environments that don't naturally meet these requirements?

---
---
---
---
---

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