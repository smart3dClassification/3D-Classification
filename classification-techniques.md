## Games as Virtual Environments

### Game Architecture Components

**Core Components:**
- **Game Core**: Represents the environment
- **AI Engine**: Computes actions
- **Physics Engine**: Implements solid state physics and classical mechanics

**Infrastructure:**
- Network layer (distributed environments)
- Spatial management
- Persistence layer

### Game State
Contains all dynamic information:
- Lists of game entities
- All attributes of game entities
- Global game information

**Not in game state:**
- Static information
- Environmental models/maps
- Preset entity attributes

### Transitions
Change valid game states into new valid states, implementing game rules.

**Triggers:**
- Player actions
- Agent actions (AI)
- Transition models (physics)

### Temporal Models

#### 1. Turn-based
- Fixed order agent querying
- Actions sequential or parallel
- Game state fixed while agents decide
- Time progresses based on slowest agent

#### 2. Transaction Systems
- Actions queued and processed FIFO
- Each transition processed sequentially
- Game state = result of last transition
- Similar to ACID database

#### 3. Tick Systems (Soft Real-Time)
- Transitions in fixed time intervals
- All actions within tick treated as simultaneous
- State transition computed jointly
- Real-time w.r.t. tick length

**Conflict Resolution for Concurrent Actions:**
- Delete both actions (undo)
- Random selection
- First action executed (natural order)

**Consistency Solutions:**
- Shadow memory (two game states G₁ and G₂)
- Fixed sequence of read/write operations

### Games as Stochastic Processes

#### Discrete Time Homogeneous Markov Process
**Definition:**
- Set of states S
- Transition function T: P(s'|s) for all s,s' ∈ S
- Start distribution B: P(s'|-)
- Terminal states S_terminal

**Example Probability Calculation:**
$$P(ACBB) = P(A|-) \cdot P(C|A) \cdot P(B|C) \cdot P(B|B) \cdot P(-|B)$$

**Properties:**
- **First-order Markov Property**: s_{t+1} only depends on s_t (memoryless)
- **Discrete time**: Process runs in discrete time steps
- **Homogeneous**: Same transition function for each time step
- **Inhomogeneous**: Transition function varies over time:
  $$p(s_{t+1} = A|s_t = B) \neq p(s_{t+i+1} = A|s_{t+i} = B) \text{ for } i \geq 1$$

### Physics Engines
**Requirements:**
- Spatial extension (polygon mesh, cylinders, MBRs)
- Movement vectors
- Mass
- Uses differential equations
- High computational cost (precomputation, numerical approximations)

### Simulation Considerations

**Event-based vs Tick-based:**
- Event-based applicable when:
  - Can schedule when agents act next
  - Can compute future environment state without agent actions
- More efficient by skipping eventless ticks

**Detail Level vs Performance Trade-offs:**
- More detail = less extended environment
- Fewer entities, lower framerates
- Detail level depends on game concept

```plaintext
Thought process

I've created comprehensive study notes from both PDFs, focusing on the technical and mathematical content while removing unnecessary fluff. The notes are organized in a clear structure with:

1. Spatial Management section covering:
   - Mathematical formulations of spatial queries
   - Performance considerations with specific calculations
   - Various spatial management techniques
   - Detailed index structures (BSP-Trees, Quad-Trees, R-Trees)
   - Algorithms in pseudocode
   - Optimization strategies

2. Games as Virtual Environments section covering:
   - Game architecture components
   - Game state management
   - Temporal models (turn-based, transaction, tick systems)
   - Games as stochastic processes with mathematical definitions
   - Physics engines and simulation considerations

All mathematical formulas, algorithms, and technical details have been preserved while removing introductory remarks, repetitive explanations, and other non-essential content.
```

I've created comprehensive study notes from both PDFs, focusing on all mathematical formulas, algorithms, and technical details while removing introductory content and redundant explanations. The notes are organized into two main sections:

1. **Spatial Management for Games** - covering spatial queries with mathematical formulations, performance analysis, index structures (BSP-Trees, Quad-Trees, R-Trees), search algorithms, and optimization techniques.

2. **Games as Virtual Environments** - covering game architecture, state management, temporal models, and games as stochastic processes with complete mathematical definitions.

All formulas, pseudocode, and technical specifications have been preserved in their original form. The notes use proper mathematical notation and include all complexity analyses and algorithmic details from the source material.




## Spatial Management for Games

### Overview
Spatial management is crucial for game scalability, involving:
- Spatial query processing for game entities
- Collision detection and area intersections
- Finding closest entities and aggro-range detection

### Spatial Queries

#### 1. ε-Range Queries
$$RQ(q, \varepsilon) = \{v \in GS | \sqrt{(q_1 - v_1)^2 + (q_2 - v_2)^2} \leq \varepsilon\}$$

#### 2. Box Query
$$BQ(q, \varepsilon) = \{v \in GS | x_1 \leq v_1 \leq y_1 \land x_2 \leq v_2 \leq y_2\}$$

#### 3. Intersection Query
$$IQ(q, r) = \{(v, s) \in GS \times \mathbb{R} | \sqrt{(q_1 - v_1)^2 + (q_2 - v_2)^2} \leq r + s\}$$

#### 4. Nearest Neighbor Query
$$NN(q) = \{v \in GS | \forall x \in GS: \sqrt{(q_1 - v_1)^2 + (q_2 - v_2)^2} \leq \sqrt{(q_1 - x_1)^2 + (q_2 - x_2)^2}\}$$

#### 5. ε-Range Join
$$S \bowtie_\varepsilon G = \{(g,s) \in G \times S | dist(g,s) \leq \varepsilon\}$$

### Performance Considerations

**Naive Approach Complexity:**
- For 1000 entities, 24 ticks/s: $\frac{1000 \times 999}{2} \times 24 = 11,988,000$ distance computations/second
- Cost grows quadratically with number of entities

### Spatial Management Techniques

#### 1. Zoning
- Splits world into fixed areas
- Only considers objects in current zone
- **Problems:**
  - Adjacent zones need consideration
  - Heterogeneous player distribution

#### 2. Micro-Zoning
- Partitions world into small areas
- Only manages entities within actual micro zone
- Can use grids or Voronoi cells
- **Issues:**
  - Zones can become overcrowded
  - Overhead for zone changes
  - Border placement affects object fluctuations

#### 3. Spatial Publish-Subscribe
- Combines micro-zoning with subscriber systems
- Entities register in current micro zone (publish)
- Entities subscribe to intersecting AoIs
- **Advantages:**
  - Efficient nearby object determination
  - Active change propagation (no regular queries needed)

### Index Structures

#### Binary Space Partitioning (BSP) Trees / kD-Trees
- **Properties:**
  - Max page capacity: M entries
  - Min page capacity: M/2 entries
  - Split along axes (alternating)
  - 50-50 data distribution
- **Bulk-Load:**
  - Creates balanced tree
  - Page in tree of height h with n objects contains:
    - At least $\lfloor\frac{n}{2^h}\rfloor$ objects
    - At most $\lfloor\frac{n}{2^h}\rfloor + 1$ objects

#### Quad-Trees
- Root = entire data space
- Each inner node has 4 successors
- Sibling nodes split parent space into 4 equal parts
- Not necessarily balanced
- No minimum filling requirement

#### R-Trees
- **Structure:**
  - Uses Minimum Bounding Rectangles (MBRs)
  - Inner nodes have m to M successors (m ≤ M/2)
  - All leaves at same height
  - MBRs can overlap

- **Insertion Algorithm:**
  1. If x contained in directory rectangle D → insert into D's subtree
  2. If x in multiple rectangles → choose smallest area
  3. If x not contained → insert into subtree with smallest area increase

- **Split Algorithm (Quadratic O(n²)):**
  - Choose pair (R₁, R₂) with largest dead space:
    $$d(R_1, R_2) = area(MBR(R_1 \cup R_2)) - area(R_1) - area(R_2)$$
  - Distribute remaining rectangles based on minimum area increase

- **Linear Split Algorithm O(n):**
  - Find rectangles with greatest normalized distance
  - Normalize by dimension extent

### Search Algorithms

#### Range Query Algorithm
```
FUNCTION List RQ(q,ε):
    List C // candidates
    List Result
    C.insert(root)
    WHILE(not C.isEmpty())
        E := C.removeFirstElement()
        IF E.isMBR()
            FOREACH F ∈ E.children()
                IF minDist(F,q) < ε
                    C.insert(F)
        ELSE
            Result.insert(E)
    RETURN Result
```

#### Nearest Neighbor Query
```
FUNCTION Object NNQuery(q):
    PriorityQueue Q // sorted by mindist
    Q.insert(0, root)
    WHILE(not Q.isEmpty())
        E := Q.removeFirstElement()
        IF E.isMBR()
            FOREACH F ∈ E.children()
                Q.insert(mindist(F,q), F)
        ELSE
            RETURN E
```

#### R-tree Spatial Join
```
FUNCTION rTreeSimJoin(R, S, result, ε)
    IF R.isDirectoryPage() or S.isDirectoryPage()
        FOREACH r ∈ R.children()
            FOREACH s ∈ S.children()
                IF minDist(r,s) ≤ ε
                    rTreeSimJoin(r,s,result,ε)
    ELSE
        FOREACH p ∈ R.points
            FOREACH q ∈ S.points
                IF dist(p,q) ≤ ε
                    result.insertPair(p,q)
    RETURN result
```

### Optimization Strategies

#### Sort-Tile Recursive (Bulk-Load for R-Trees)
1. Calculate quantile: $q = \lceil\sqrt{\frac{n}{M}}\rceil$
2. Sort data in dimension 1
3. Generate quantile after q·M objects
4. Sort each quantile in dimension 2
5. Create MBR around points in each cell
6. Restart with derived MBRs until q < 2

**Time Complexity:** O(n log n)

#### Throw-Away Indices
- For highly volatile data, rebuilding with bulk-load is more efficient than updating
- Use two index structures:
  - I₁: Last consistent tick (for queries)
  - I₂: Created simultaneously for next tick
- Swap indices at tick boundary

### Game Design Considerations
Spatial problems depend on:
- Number and distribution of spatial objects
- Number and distribution of players
- Environmental model (2D/3D)
- Movement type and speed of objects

---







## Distributed Environments

### Distributed Architectures

#### 1. Client-Server
- Provider hosts game in computing center
- Centralized solution for:
  - Account management
  - Game world partitioning
  - Monitoring
  - Persistence

#### 2. Multi-Server
- Several servers with redundant data storage
- Shorter network distance client-server
- Dynamic solutions:
  - Replication
  - Proxy servers

#### 3. Peer-to-Peer
- No explicit servers
- Data exchange between adjacent peers
- Every peer hosts part of game world
- Dynamic partitioning

### Brewer's CAP Theorem
> Any networked shared-data system can have at most two of three properties:
- **Consistency**: All nodes see same data
- **Availability**: System remains operational
- **Partition Tolerance**: System continues despite network failures

### Protocol Content Types

#### 1. Object Attributes (Action Result Protocol)
- Sets current parameter values
- Sends relative changes
- Example: `set player HP to 96` or `reduce player HP by 100`

#### 2. Actions (Action Request Protocol)
- Contains only player input
- No direct impact on game state
- Results calculated on server
- Example: `try to hit Player with Uppercut`

### Distributed Action Handling

#### Thin Client Solution
**Process:**
- Server holds complete game state
- Clients receive partial state on login
- Server transmits state changes to clients
- Clients send action requests to server
- Server handles actions by arrival order

**Advantages:**
- Centrally managed consistent state
- Low cheating potential
- Consistent persistence

**Disadvantages:**
- Maximum server load
- High latencies (round trip times)
- Unused client processing power

#### Fat Client Solution
**Process:**
- Every client owns objects only it can edit
- Server manages chronological sequence with timestamps
- Local game states may vary due to transmission delay

**Conflict Resolution:**
- Client maintains 2 game states:
  - Optimistic GS (contains local changes)
  - Pessimistic GS (contains server-transmitted actions)
- On mismatch: reset optimistic to pessimistic

### Dead Reckoning

**Definition**: Simulating movement between updates for fluid movement with limited bandwidth

#### Components:
1. **Update Strategy** (server side)
   - Controls when/how often position info transmitted
   - Influences bandwidth and error rate

2. **Movement Model** (remote peer)
   - Methods to extrapolate fluid movement
   - Influences error rate and perception

3. **Error Correction** (remote peer)
   - Converges extrapolated and exact positions
   - Influences perception quality

#### Movement Models

**Linear movement with constant speed:**
```
p(t₁ + t_Δ) = p(t₁) + (p(t₁) - p(t₀))/||p(t₁) - p(t₀)|| × t_Δ × ||p(t₁) - p(t₀)||/(t₁ - t₀)
```

**Linear movement with constant acceleration:**
```
p(tᵢ + t_Δ) = ½a(tᵢ)t²_Δ + v(tᵢ)t_Δ + p(tᵢ)

where:
a(tᵢ) = Δv/Δt ≈ (v(tᵢ) - v(tᵢ₋₁))/(tᵢ - tᵢ₋₁)
v(tᵢ) = Δp/Δt ≈ (p(tᵢ) - p(tᵢ₋₁))/(tᵢ - tᵢ₋₁)
```

#### Update Strategies

1. **Regular updates**: Send at fixed intervals
2. **Event-based updates**: Send on direction/movement type change
3. **Distance-based updates**: Variable rate based on distance/proximity to critical range

#### Error Correction - Hermite Splines

**Base polynomials:**
- h₁(x) = 2x³ - 3x² + 1
- h₂(x) = -2x³ + 3x²
- h₃(x) = x³ - 2x² + x
- h₄(x) = x³ - x²

**Connecting formula:**
```
p(x) = p(t)h₁(x) + p'(t+Δt)h₂(x) + d(t)h₃(x) + d'(t)h₄(x)
```
where 0 ≤ x ≤ 1

### Network Requirements for Games

**Bandwidth requirements by genre:**

| Game | Avg payload (bytes) | Avg bandwidth (bps) |
|------|-------------------|-------------------|
| Anarchy Online | 98 | 2168 |
| World of Warcraft | 26 | 2046 |
| Counter Strike | 36 | 19604 |
| Halo 3 | 247 | 60223 |

**Latency requirements:**
- RTS games: < 1000 ms
- RPG: < 500 ms
- FPS: < 100 ms

### Network Protocols

#### TCP/IP
**Advantages:**
- Safe protocol (re-transmission)
- Flow control and congestion control
- Optimized for bandwidth usage

**Disadvantages:**
- Packages may arrive with significant delay
- Increased latency from retransmission
- Outdated information still transmitted
- Bandwidth optimization increases latency

#### UDP
**Advantages:**
- No retransmission of lost packages
- Outdated information not resent
- Less header overhead
- Minimal datagram service

**Disadvantages:**
- Unsafe transmission
- No sequence guarantee
- No congestion control

**Usage:**
- Middleware solutions implement missing features
- Maintain update sequence
- Security for critical operations

### Key Takeaways

1. **Persistence Trade-offs**:
   - Naive Snapshot: Best for volatile systems with many changes
   - Wait-Free methods: Better for fewer updates
   - Ping-Pong: Reduces overhead but uses more memory

2. **Distribution Trade-offs**:
   - Latency vs Consistency (CAP theorem)
   - Server-side processing: Accuracy over response time
   - Client-side processing: Response time over consistency

3. **Network Considerations**:
   - TCP still most used despite latency issues
   - UDP for just-in-time services (movement, voice)
   - Secure services must be implemented at application layer




## Persistence

### Overview
Persistence in games involves saving game states and managing replays. Key components:
- Save games: saving part of current game state (gs ∈ GS)
- Replays: saving game progression (gs₁, ..., gsₑₙ𝒹)

### Requirements for Persistence Layers

1. **Performance Requirements**
   - Saving should not slow down the game
   - Tick duration must not exceed time limit
   - Save games should be as up-to-date as possible
   - If possible, GS should be saved every tick

2. **Consistency Requirements**
   - Loading must create consistent state
   - All game entities should be equally up-to-date
   - Minimum: game state must be consistent (every GE appears only once)

3. **Impact Factors**
   - Size of game state
   - Requirements on recency and tick frequency
   - Impact of loading time on recovery
   - Part of game state/history to be saved

### Logging Methods

#### 1. State-Log
- **Definition**: Every game entity saved every x ticks
- **Format**: Sequential file containing series of game states
- **Example**: Quake/Half-life/Counterstrike demo files

**Advantages:**
- Documents genuine series of game states
- Random access to every point in time
- Simple and fast loading process

**Disadvantages:**
- High redundancy for small change rates
- Large data volumes due to high temporal resolution
- Maximum writing load (possibly not feasible for large game states)

#### 2. Transition-Log
- **Definition**: Log all changes to game states
- **Components**:
  - Time-stamp
  - ID of game entity
  - Attribute
  - New value

**Advantages:**
- More compact than snapshots
- Less volume = less computational effort

**Disadvantages:**
- Reconstructing game states is more complex
- All changes must be registered at persistence layer

#### 3. Action-Log
- **Definition**: Contains sequence of all user inputs
- **Requirements**:
  - Game needed to "re-play" based on user input
  - Random events must be saved (seeding or random numbers)
- **Example**: Starcraft II replay files

**Advantages:**
- Replays can be more compact (APM vs ticks per second)
- No redundancy
- May contain more information than game state

**Disadvantages:**
- Restoring last state expensive (rerunning entire game)
- Hard for large numbers of random elements
- Requires deterministic AI behavior

### Persistence in Large Distributed Environments

**Using Relational Databases:**

Advantages:
- Consistency checks (no duplicates)
- Selective requests with efficient indexes
- Current game state immediately available
- Innate recovery mechanisms

Disadvantages:
- Structured saving increases processing times

**Using Log-files:**

Advantages:
- Less system overhead
- Minimal waiting time (sequential writes)

Disadvantages:
- No protection from hardware errors
- Selective requests not supported
- Loading may require extensive reconstructions

### Checkpoint Recovery Methods

#### 1. Naive Snapshot
- **Process**: Copy whole game state into shadow memory when write-thread finished
- **Timing**: Copy at end of checkpoint, write at start of next tick

**Advantages:**
- No overhead from locking or bit-resets
- Efficient for large numbers of changes

**Disadvantages:**
- Large overhead for limited changes
- May cause lags during copying

#### 2. Copy-on-Update (Selective Copy)
- **Process**: Each object has dirty bit set on change
- **End of checkpoint**: Copy dirty objects to shadow, reset bits

**Advantages:**
- Only writes changed objects

**Disadvantages:**
- Still high effort when periods end

#### 3. Wait-Free Zigzag
- **Components**: Two flags per object - MW (Write-State) and MR (Read-State)
- **Update process**: 
  - New value set in GS[MWᵢ]
  - MRᵢ set to MWᵢ
  - Writer reads from GS[MWᵢ]
  - End of period: if MWᵢ equals MRᵢ, flip MWᵢ

**Advantages:**
- No locking necessary
- Changes written over time

**Disadvantages:**
- Still requires bit-reset at end of each period

#### 4. Wait-Free Ping-Pong
- **Uses 3 game states**:
  - Action handling (GS)
  - Persistence-system (read)
  - Persistence-system (write)
- **Process**: Updates in GS and write state, writer reads from read state
- **Period change**: Swap write and read states

**Advantages:**
- No locking or bit-reset needed

**Disadvantages:**
- Triple memory requirements
