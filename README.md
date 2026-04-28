# 🏕️ Picnic Quest — Game Theory Board Game

---

# 📌 Summary

### 🔹 Brief Overview

Picnic Quest is an interactive board game powered by **game theory and advanced algorithms**. It combines a visually rich UI with a high-performance C++ engine to simulate optimal decision-making using the **Sprague–Grundy theorem** along with classical algorithms like BFS, Dijkstra, Minimax, Binary Search, Prefix Sum, and Bitmasking.

---

### 🎯 Objectives

* Build a strategic board game using **combinatorial game theory**
* Implement multiple **core algorithms in a real-world system**
* Provide **AI-driven decision support**
* Visualize algorithm outputs in an intuitive UI
* Compare multiple AI strategies through simulation

---

# 📖 Introduction

### 🔹 Background and Context

Traditional board games rely on randomness and heuristics. This project introduces a **mathematically optimal approach** using the Sprague–Grundy theorem, transforming gameplay into a **state-based decision system**.

---

### 🔹 Problem Statement

Design a system where:

* Each game state is analyzed mathematically
* Optimal decisions are computed in real time
* Multiple algorithms contribute to strategy evaluation
* Users can visualize and understand algorithmic impact

---

### 🔹 Project Objectives

* Model the board game as a **state graph**
* Implement **Grundy-based winning/losing state detection**
* Integrate **AI decision-making strategies**
* Provide **real-time analytics dashboard**
* Build a seamless **Python–C++ hybrid system**

---

### 🔹 Scope of the Project

* Two-player board game (Human vs Human / AI)
* Algorithm visualization platform
* AI strategy comparison system
* Educational tool for understanding algorithms

---

# 💻 System Requirements

### 🔹 Functional Requirements

* Player movement based on dice rolls
* Cell-based actions (gain/loss, jump, skip, etc.)
* Grundy value computation for current state
* AI move recommendation system
* Algorithm dashboard (BFS, Dijkstra, etc.)
* Game reset and restart functionality
* Real-time logging system

---

# 🛠️ Design and Implementation

---

## 📊 UML Diagrams (Conceptual)

### State Representation

```
State
 ├── position (0–35)
 ├── money (0–30)
 ├── dice (6 / 12)
 └── items (bitmask)
```

### System Architecture

```
[ Streamlit UI ]
        ↓
[ Python Bridge ]
        ↓
[ C++ Game Engine ]
        ↓
[ Algorithms Execution ]
```

---

## ⚙️ Algorithms Implementation (Detailed)

---

### 1. 🧠 Sprague–Grundy Theorem (Core Algorithm)

#### ✔ Purpose

Determines whether a given game state is:

* **Winning (G ≠ 0)**
* **Losing (G = 0)**

---

#### ✔ State Definition

Each game state is defined as:

```
(pos, money, die)
```

* `pos` → current cell
* `money` → affects reset condition
* `die` → 6-sided or 12-sided

---

#### ✔ Implementation Logic

1. **Base Case**

```
If pos == finish → Grundy = 0
```

2. **Generate all possible moves**

```
for r = 1 → dice_faces:
    next_state = apply_cell(pos + r)
```

3. **Recursive computation**

```
Compute Grundy for all next states
```

4. **Apply mex**

```
Grundy(state) = mex(all next Grundy values)
```

---

#### ✔ mex (Minimum Excluded Value)

* Smallest non-negative integer NOT in the set

Example:

```
[0,1,2] → mex = 3
[1,2,3] → mex = 0
```

---

#### ✔ Key Insight

* If **any move → Grundy = 0** → current state is winning
* If **all moves → non-zero** → losing

---

#### ✔ Optimization (DP)

* Memoization used:

```
unordered_map<int,int> g_memo;
```

* Avoids recomputation of states

---

#### ✔ Complexity

```
O(states × transitions)
≈ O(36 × 30 × 2 × 12)
```

---

---

### 2. 🔍 Binary Search (Minimum Coins for Guaranteed Win)

#### ✔ Purpose

Find minimum coins such that:

```
Grundy(pos, coins, die) ≠ 0
```

---

#### ✔ Implementation Logic

1. Search in range:

```
[1, MAX_MONEY]
```

2. Check:

```
canWin(pos, mid, die)
```

3. Adjust:

* If winning → search left
* Else → search right

---

#### ✔ Why it works

* Winning condition becomes **monotonic** w.r.t money

---

#### ✔ Complexity

```
O(log M × Grundy)
```

---

---

### 3. 📊 Prefix Sum (Range Money Queries)

#### ✔ Purpose

Efficiently compute total gain/loss between cells

---

#### ✔ Implementation

**Build:**

```
PREFIX[i+1] = PREFIX[i] + BOARD[i].value
```

**Query:**

```
sum(L,R) = PREFIX[R+1] - PREFIX[L]
```

---

#### ✔ Complexity

* Build → O(N)
* Query → O(1)

---

---

### 4. 🌐 BFS (Minimum Moves to Finish)

#### ✔ Purpose

Compute minimum number of dice rolls to reach finish

---

#### ✔ Key Idea

Use **reverse graph traversal**

---

#### ✔ Implementation

1. Construct reverse edges:

```
dst → src
```

2. BFS from finish:

```
dist[finish] = 0
```

3. Expand backwards

---

#### ✔ Complexity

```
O(V + E)
```

---

---

### 5. ⚡ Dijkstra (Minimum Risk Path)

#### ✔ Purpose

Find safest path instead of shortest

---

#### ✔ Risk Function

Assign cost based on cell type:

* Money loss → high cost
* Skip turn → very high
* Gain → low

---

#### ✔ Implementation

1. Build weighted graph
2. Use priority queue
3. Relax edges

---

#### ✔ Complexity

```
O(E log V)
```

---

---

### 6. 🤖 Minimax (Adversarial AI)

#### ✔ Purpose

Simulate opponent and choose optimal move

---

#### ✔ Logic

* Maximizer → maximize score
* Minimizer → minimize score

---

#### ✔ Recursion

```
minimax(state, depth, player)
```

---

#### ✔ Base Case

* Depth = 0
* Or reached finish

---

#### ✔ Complexity

```
O(branch^depth)
```

---

---

### 7. 🎒 Bitmasking (Item Tracking)

#### ✔ Purpose

Efficiently track collected items

---

#### ✔ Representation

```
int mask
```

Each bit = one item

---

#### ✔ Operations

Check:

```
(mask >> i) & 1
```

Add:

```
mask |= (1 << i)
```

---

#### ✔ Complexity

```
O(1)
```

---

---

## 📈 Algorithms and Their Complexities

| Algorithm     | Complexity              |
| ------------- | ----------------------- |
| Grundy (DP)   | O(states × transitions) |
| Binary Search | O(log M × Grundy)       |
| Prefix Sum    | O(N) build, O(1) query  |
| BFS           | O(V + E)                |
| Dijkstra      | O(E log V)              |
| Minimax       | O(branch^depth)         |
| Bitmask       | O(1)                    |


---

## 💻 Code Snippets

### 🔹 Grundy Function (with DP)

```cpp
int grundy(int pos,int money,int die){
    if(pos >= BOARD_SIZE-1) return 0;

    int key = enc(pos,money,die);
    if(g_memo.count(key)) return g_memo[key];

    int faces = (die==0)?6:12;
    vector<int> next;

    for(int r=1;r<=faces;r++){
        int np = pos + r;
        if(np > BOARD_SIZE-1) continue;

        State ns = apply_cell(np,money,die);
        next.push_back(grundy(ns.pos,ns.money,ns.die));
    }

    return g_memo[key] = mex(next);
}
```

---

### 🔹 Binary Search

```cpp
int binarySearchMinCoins(int pos,int die){
    int lo=1,hi=MAX_MONEY,ans=MAX_MONEY;

    while(lo<=hi){
        int mid=(lo+hi)/2;
        if(canWin(pos,mid,die)){
            ans=mid;
            hi=mid-1;
        } else lo=mid+1;
    }
    return ans;
}
```

---

### 🔹 BFS

```cpp
vector<int> bfs_min_moves(){
    vector<int> dist(N,INT_MAX);
    queue<int> q;

    dist[target]=0;
    q.push(target);

    while(!q.empty()){
        int u=q.front(); q.pop();

        for(int v:rev[u]){
            if(dist[v]==INT_MAX){
                dist[v]=dist[u]+1;
                q.push(v);
            }
        }
    }
    return dist;
}
```

---

### 🔹 Bitmask

```cpp
mask |= (1 << item_id);
```

---

---

# ✅ Conclusion

### 🔹 Summary & Achievement of Objectives

* Successfully implemented a **game theory-based board game**
* Integrated multiple algorithms into a single system
* Built a **real-time decision analysis tool**
* Achieved seamless **Python–C++ integration**
* Provided strong **visualization and user interaction**

---

### 🔹 Future Work and Recommendations

* Add multiplayer (online)
* Improve AI using reinforcement learning
* Expand board size and complexity
* Add difficulty levels
* Optimize Grundy computation further
* Deploy as a web application

---
