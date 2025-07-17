## Part 1: Virtual Environments

---
---
---
---
---

What are the four main architecture layers in game development from bottom to top? What types of components does the Game Engine Layer contain?

---
---
---
---
---

### 1. Building Blocks in Architecture

Games are built using a layered architecture with the following components:

#### Architecture Layers (Bottom to Top):
1. **Hardware Layer**: Sound Card, Graphics Card, Input devices, Network
2. **Hardware Abstraction Layer**: DirectX, OpenGL, etc.
3. **Game Engine Layer**: Contains multiple subsystems
4. **Game Content Layer**: The actual game assets and logic

#### Game Engine Components:

**Core Systems:**
- **Game Core**: Represents and manages the environment
- **Persistence Layer**: Stores data for analysis and learning
- **Physics Engine**: Simulates physical interactions

**Media Systems:**
- **Sound Manager**: Audio processing
- **Animation Engine**: Character and object animations
- **Rendering Engine**: Visual output generation
- **Graphics Engine**: Low-level graphics operations

**Support Systems:**
- **User Interface**: Player interaction
- **Script Language**: Game logic scripting
- **AI Engine**: Computes NPC actions
- **Network Module**: Multiplayer support

#### AI-Relevant Components:
- **Directly involved:**
  - AI Engine: Computes actions
  - Game Core: Represents the environment
  - Physics Engine (sometimes)
  
- **Supporting infrastructure:**
  - Network layer: For distributed environments
  - Spatial management: In core or physics engine
  - Persistence layer: Store data for analysis/learning
  - Scalability considerations for MMO or fast simulations

---
---
---
---
---

What does Game State contain and what types of information are explicitly NOT part of Game State? How does the Model-View-Controller pattern apply to game architecture?

---
---
---
---
---

### 2. Game State

The **Game State** contains all dynamic information representing the current state of the game:

#### Components:
- **Objects, attributes, relationships** (think ER or UML models)
- **Dynamic information only**
- **Lists of all game entities**
- **Attributes of game entities**
- **Global game information**

#### Not in Game State:
- Static information
- Environmental models/maps (unless dynamic)
- Preset attributes of game entities

#### Design Pattern: Model-View-Controller (MVC)
- **Server (Model)**: Manages game state, no visualization needed
- **Client (View + Controller)**: Partial game state, handles I/O and visualization

**Example - Chess Game State:**
```
Game Information:
- Players and colors (black/white)
- Game mode (with/without clock)

Game State:
- Board positions (8x8 grid)
- Current player's turn
- Time remaining (if clock mode)
```

---
---
---
---
---

What are the three main triggers for state changes in games? What key properties must transitions maintain?

---
---
---
---
---

### 3. Transitions

Transitions change the game state over time.

#### Triggers for State Changes:
1. **Player actions** (e.g., chess move)
2. **Agent actions** (AI decisions)
3. **Transition models** (e.g., gravity causing objects to fall)

#### Key Properties:
- Transitions maintain **consistency**: valid state → valid state
- Transitions **implement game rules**
- Need temporal synchronization

**Example - Chess Transition:**
```
Initial State: Knight at G1
Action: Move Knight to F3
Validation: Check if move is legal (L-shaped pattern)
New State: Knight at F3
```

---
---
---
---
---

What parameters do physics engines require from game entities? What is the fundamental trade-off between detail level and performance in transition modeling?

---
---
---
---
---

### 4. Transition Modeling

#### Physics Engines:
- Implement solid state physics and classical mechanics
- Required parameters from game entities:
  - Spatial extension (mesh, cylinders, bounding boxes)
  - Movement vectors
  - Mass
  - Other physical properties
  
- Use differential equations
- Require high tick rates for realism
- Computationally expensive (precomputation, approximations)

#### Detail Level vs Performance Trade-off:
- **High detail**: More accurate physics, fewer entities, lower framerates
- **Low detail**: Approximations, more entities, higher framerates

**Example:**
- FPS: Models bullet physics precisely
- RPG: Combat as statistical random process

---
---
---
---
---

What are the three main temporal models for organizing when transitions occur? How do they differ in terms of when and how agent actions are processed?

---
---
---
---
---

### 5. Temporal Models

Three main temporal models organize when transitions occur:

---
---
---
---
---

In a turn-based model, what prevents blocking and how do agents interact with the game state during decision-making? What are the main advantages and disadvantages?

---
---
---
---
---

#### 5.1 Turn-Based Model
- **Process**: Agents queried in fixed order
- **Properties**:
  - Game state fixed during decision-making
  - Actions sequential or parallel
  - Agent timeout prevents blocking
  
**Example - Round-based RPG:**
```
1. Environment determines attack order
2. Agent 1 chooses action → compute new state
3. Agent 2 chooses action → compute new state
4. Environment progresses transition model
```

**Pros:**
- Clear, understandable timing
- Speed adjusts to slowest agent

**Cons:**
- No real-time synchronization
- Fast agents can't gain advantage

---
---
---
---
---

How does the transaction model process actions and what database concept does it resemble? What advantage do faster agents have in this model?

---
---
---
---
---

#### 5.2 Transaction Model
- **Process**: Actions queued FIFO
- **Properties**:
  - Each transition processed sequentially
  - Like ACID database transactions
  - Agents submit actions at any speed

**Pros:**
- Guarantees valid state
- Faster agents → more actions

**Cons:**
- No wall-clock synchronization
- No true simultaneous actions

---
---
---
---
---

What makes tick systems "soft" real-time and how are actions within a tick treated? What are the two main consistency solutions for tick systems?

---
---
---
---
---

#### 5.3 Tick Systems (Soft Real-Time)
- **Process**: Fixed time intervals
- **Properties**:
  - Actions within tick treated as simultaneous
  - Real-time with respect to tick length
  - "Soft" = processing may exceed tick duration

**Example - Concurrent Actions:**
```
Character A: 100 HP
At tick t:
- Takes 100 damage from B
- Receives 100 healing from C
Result: Effects cancel (not sequential processing)
```

**Pros:**
- Synchronizes game-time with wall-clock
- Fair action rates
- True concurrency

**Cons:**
- Lag handling
- Conflict resolution needed
- Chronological ordering within ticks

#### Consistency Solutions:
1. **Shadow Memory**:
   - Two game states: G1 (active), G2 (inactive)
   - Read from G1, write to G2
   - Swap at tick end
   
2. **Fixed Read/Write Sequence**:
   - Structured action processing
   - Prevents read-after-write issues

---
---
---
---
---

Why can games be modeled as Markov processes? What are the key components of a Discrete-Time Homogeneous Markov Process and what does the Markov property mean?

---
---
---
---
---

### 6. Stochastic Processes

Games can be modeled mathematically as Markov processes due to non-deterministic elements.

#### Discrete-Time Homogeneous Markov Process:
- **States**: S = {all possible game states}
- **Transition function**: T: P(s'|s) for all s,s' ∈ S
- **Start distribution**: B: P(s'|-) 
- **Terminal states**: S_terminal

**Example Transition Matrix:**
```
      -    A     B     C
-   0.0  0.3   0.3   0.4
A   0.1  0.25  0.5   0.15
B   0.1  0.5   0.4   0.0
C   0.1  0.1   0.7   0.1
```

**Probability Calculation:**
```
P(ACBB) = P(A|-) × P(C|A) × P(B|C) × P(B|B) × P(-|B)
        = 0.3 × 0.15 × 0.7 × 0.4 × 0.1
```

#### Properties:
- **Markov Property**: Next state depends only on current state (memoryless)
- **Discrete time**: Turn-based or tick-based
- **Homogeneous**: Same transition function over time (usually)
- Games fulfill Markov property: game state contains all needed information

## Part 2: Spatial Management

---
---
---
---
---

What are the four key spatial queries that games need to perform efficiently? Using the example of 1000 entities at 24 ticks/second, how many distance calculations would a naive approach require per second?

---
---
---
---
---

### 1. Spatial Management Overview

Most games occur in spatial environments requiring efficient spatial query processing.

#### Key Spatial Queries:
1. **Area of Interest (AoI)**: Which entities are within interaction range?
2. **Collision Detection**: Pre-filtering for detailed physics
3. **Nearest Neighbor**: Find closest entity
4. **Range Checks**: Aggro range, attack range

**Scalability Problem:**
```
1000 entities, 24 ticks/second
Naive approach: (1000 × 999)/2 × 24 = 11,988,000 distance calculations/second
```

---
---
---
---
---

What are the four main types of spatial queries and their mathematical definitions? How does an intersection query differ from a range query?

---
---
---
---
---

### 2. Spatial Query Types

#### 2.1 ε-Range Query
```
RQ(q,ε) = {v ∈ GS | √((q₁-v₁)² + (q₂-v₂)²) ≤ ε}
```
Returns all objects within distance ε from query point q.

#### 2.2 Box Query
```
BQ(q,ε) = {v ∈ GS | x₁ ≤ v₁ ≤ y₁ ∧ x₂ ≤ v₂ ≤ y₂}
```
Returns all objects within rectangular region.

#### 2.3 Intersection Query
```
IQ(q,r) = {(v,s) ∈ GS × ℝ | √((q₁-v₁)² + (q₂-v₂)²) ≤ r + s}
```
Finds objects whose ranges overlap with query range.

#### 2.4 Nearest Neighbor Query
```
NN(q) = {v ∈ GS | ∀x ∈ GS: dist(q,v) ≤ dist(q,x)}
```
Returns closest object to query point.

---
---
---
---
---

What are the three main spatial management techniques? What specific problems does basic zoning face?

---
---
---
---
---

### 3. Spatial Management Techniques

---
---
---
---
---

How does zoning split the game world and what are its main benefits? What challenges arise with border objects and player distribution?

---
---
---
---
---

#### 3.1 Zoning
- **Concept**: Split world into fixed areas
- **Benefits**: 
  - Only check objects in current zone
  - Easy distribution across servers
- **Problems**:
  - Border objects need multi-zone checks
  - Heterogeneous player distribution

---
---
---
---
---

How does micro-zoning differ from regular zoning? If an entity is at position (25, 35) with zone size 10x10 and has an AoI radius of 15, which zones need to be checked?

---
---
---
---
---

#### 3.2 Micro-Zoning
- **Concept**: Many small zones (grid/Voronoi cells)
- **Process**:
  - Entities managed in their micro-zone
  - Check zones intersecting AoI
  - Sequential search within zones

**Grid-based Example:**
```
Zone size: 10x10 units
Entity at (25, 35) → Zone (2,3)
AoI radius 15 → Check zones (1-3, 2-4)
```

---
---
---
---
---

What is the three-step process in spatial publish-subscribe? What are the main advantages and disadvantages of this approach?

---
---
---
---
---

#### 3.3 Spatial Publish-Subscribe
- **Concept**: Combine micro-zoning with pub/sub
- **Process**:
  1. Entities publish to their zone
  2. Subscribe to zones in AoI
  3. Receive updates on zone changes
  
**Advantages:**
- Efficient proximity detection
- Active change notification
- No polling needed

**Disadvantages:**
- Zone size trade-offs
- Border fluctuations
- High change rates = overhead

---
---
---
---
---

What are the three main types of index structures used for spatial management? How do they differ in their approach to space partitioning?

---
---
---
---
---

### 4. Index Structures

---
---
---
---
---

What is a kD-Tree and how does it alternate split dimensions? What are the maximum and minimum capacity requirements for nodes, and what are the pros and cons of bulk loading?

---
---
---
---
---

#### 4.1 Binary Space Partitioning (BSP) Trees

**kD-Tree (most common BSP):**
- Binary tree with axis-aligned splits
- Alternates split dimension at each level
- Leaf nodes contain objects

**Properties:**
- Max capacity: M entries
- Min capacity: M/2 entries
- Split on overflow (50-50 distribution)

**Bulk Loading Algorithm:**
```python
def bulk_load_kd_tree(points, M):
    if len(points) <= M:
        return LeafNode(points)
    
    # Alternate dimensions
    dim = depth % num_dimensions
    
    # Sort and split at median
    points.sort(key=lambda p: p[dim])
    mid = len(points) // 2
    
    left = bulk_load_kd_tree(points[:mid], M)
    right = bulk_load_kd_tree(points[mid:], M)
    
    return InnerNode(left, right, split_value)
```

**Pros:**
- Balanced tree from bulk load
- Guaranteed bounds: n/2^h to n/2^(h+1) objects per node

**Cons:**
- No dynamic balancing
- Can degenerate with updates

---
---
---
---
---

How many children does each node in a Quad-Tree have? What are the key differences between Quad-Trees and kD-Trees in terms of structure and balance?

---
---
---
---
---

#### 4.2 Quad-Trees
- Each node has 4 children
- Space divided into equal quadrants
- No minimum fill requirement

**Structure:**
```
         Root
    /    |    |    \
  NW    NE    SW    SE
```

**Properties:**
- Not necessarily balanced
- Simple spatial subdivision
- Good for uniform distributions

---
---
---
---
---

What are MBRs in R-Trees and can they overlap? How does the insertion algorithm choose which subtree to insert into, and what is the quadratic split algorithm based on?

---
---
---
---
---

#### 4.3 R-Trees

**Structure:**
- Balanced tree (all leaves at same height)
- Nodes have m to M children (m ≤ M/2)
- Page regions are Minimum Bounding Rectangles (MBRs)
- MBRs may overlap

**Insertion Algorithm:**
```python
def insert(node, object):
    if node.is_leaf():
        node.add(object)
        if node.overflow():
            split(node)
    else:
        # Choose subtree
        best_child = None
        if object in multiple MBRs:
            best_child = smallest_area_child
        elif object not in any MBR:
            best_child = min_area_increase_child
        else:
            best_child = containing_child
        
        insert(best_child, object)
        adjust_mbr(node)
```

**Split Algorithm (Quadratic):**
1. Find pair (R1,R2) with maximum dead space
   ```
   d(R1,R2) = area(MBR(R1∪R2)) - area(R1) - area(R2)
   ```
2. Assign remaining objects to minimize area increase

**Linear Split (O(n)):**
- Find pair with maximum normalized distance
- Faster but potentially worse quality

**Sort-Tile-Recursive Bulk Load:**
```python
def sort_tile_recursive(objects, M):
    q = ceil(sqrt(len(objects)/M))
    
    # Sort by dimension 1
    objects.sort(key=lambda o: o.x)
    
    # Create q slices
    for i in range(q):
        slice = objects[i*q*M:(i+1)*q*M]
        # Sort slice by dimension 2
        slice.sort(key=lambda o: o.y)
        
        # Create M-sized pages
        for j in range(0, len(slice), M):
            create_page(slice[j:j+M])
```

---
---
---
---
---

What are the two main search algorithms for spatial queries? How does the nearest neighbor query use a priority queue differently from the range query?

---
---
---
---
---

### 5. Search Algorithms

#### 5.1 Range Query
```python
def range_query(root, q, epsilon):
    candidates = [root]
    results = []
    
    while candidates:
        node = candidates.pop()
        if node.is_mbr():
            for child in node.children:
                if min_dist(child, q) <= epsilon:
                    candidates.append(child)
        else:  # Leaf/object
            results.append(node)
    
    return results
```

#### 5.2 Nearest Neighbor Query
```python
def nn_query(root, q):
    # Priority queue ordered by min distance
    pq = PriorityQueue()
    pq.insert(0, root)
    
    while not pq.empty():
        dist, node = pq.pop()
        if node.is_object():
            return node  # First object is nearest
        else:
            for child in node.children:
                pq.insert(min_dist(child, q), child)
```

---
---
---
---
---

What is a spatial join and how does it compute all AoIs in a single operation? How does the R-Tree join algorithm work recursively?

---
---
---
---
---

### 6. Spatial Joins

**ε-Range Join Definition:**
```
S ⋈_ε G = {(g,s) ∈ G×S | dist(g,s) ≤ ε}
```

Computes all AoIs in single operation.

**R-Tree Join Algorithm:**
```python
def r_tree_join(R, S, epsilon):
    results = []
    
    if R.is_leaf() and S.is_leaf():
        for r in R.objects:
            for s in S.objects:
                if dist(r, s) <= epsilon:
                    results.append((r, s))
    else:
        for r_child in R.children:
            for s_child in S.children:
                if min_dist(r_child, s_child) <= epsilon:
                    results.extend(
                        r_tree_join(r_child, s_child, epsilon)
                    )
    
    return results
```

---
---
---
---
---

Why do traditional index structures struggle with high volatility in games? What is the throw-away indices solution and what is the decision criteria for using an index?

---
---
---
---
---

### 7. Handling High Volatility

#### Problem:
- Game objects move frequently (every tick)
- Traditional index updates are expensive
- Structures can degenerate

#### Solution: Throw-Away Indices
1. Maintain two indices: I₁ (active), I₂ (building)
2. Use I₁ for queries while building I₂
3. Swap indices each tick
4. Rebuild via bulk-load (fast, good structure)

**Decision Criteria:**
```
Use index if: 
  Time(bulk_load) + Time(index_queries) < Time(brute_force_queries)
```

---
---
---
---
---

What are the main trade-offs in spatial management performance? What are four optimization strategies and how do game design decisions impact spatial management?

---
---
---
---
---

### 8. Performance Considerations

#### Trade-offs:
- **Update frequency** vs **Query performance**
- **Index quality** vs **Build time**
- **Memory usage** vs **Speed**

#### Optimization Strategies:
1. **Reduce queries**: Lower tick rates for some checks
2. **Spatial publish-subscribe**: Avoid repeated queries
3. **Hierarchical approaches**: Coarse then fine checks
4. **Parallel processing**: Spatial joins for batch operations

#### Game Design Impact:
- Object count and distribution
- Player distribution patterns
- Environment type (2D/3D)
- Movement patterns and speeds

---
---
---
---
---

What four key factors determine the choice of spatial management technique? What is the key principle regarding update/build time and query processing?

---
---
---
---
---

### Summary

Efficient spatial management is crucial for game scalability. The choice of technique depends on:
- **Static vs Dynamic**: How often do objects move?
- **Distribution**: Uniform or clustered?
- **Query patterns**: What queries are most frequent?
- **Performance requirements**: Real-time constraints?

Key principle: Update/build time must be compensated by query processing speedups. For highly dynamic games, throw-away indices with bulk loading often outperform traditional dynamic structures.
