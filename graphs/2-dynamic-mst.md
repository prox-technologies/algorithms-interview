# Part 2: Dynamic MST

You now know how to build an MST from scratch. But what happens when the graph changes? This is the core of what we're testing: given an existing MST, how do you update it when edges are added, nodes arrive, or weights change?

Everything here follows from the cut-cycle duality you learned in Part 1.

---

## Edge Insertion

**Problem:** You have an MST $`T`$ of graph $`G`$. A new edge $`e = (u, v)`$ with weight $`w(e)`$ is added. Find the new MST.

**Algorithm:**
1. Add $`e`$ to $`T`$. This creates exactly one cycle (since $`T`$ was a tree).
2. Find the heaviest edge $`e^*`$ in that cycle.
3. If $`e^* = e`$: the new edge is useless. MST unchanged.
4. If $`e^* \ne e`$: remove $`e^*`$, keep $`e`$. New MST is $`T' = T - e^* + e`$.

**Why it works:** The cycle property says the unique heaviest edge in a cycle can't be in any MST. Every other edge that was previously not in $`T`$ still has a cycle in $`T`$ where it's the heaviest (that's why it wasn't in $`T`$ before), and those cycles still exist. So only edges in $`T \cup \{e\}`$ can be in the new MST.

---

## Node Insertion

**Problem:** You have an MST $`T`$ of graph $`G`$. A new vertex $`v'`$ arrives with edges $`E'`$ to existing vertices. Find the new MST.

**Key insight:** Only the edges in $`T \cup E'`$ can be in the new MST. Why? Every edge $`e' \in E \setminus T`$ (an old non-tree edge) was the heaviest in some cycle in $`T`$. That cycle still exists in the new graph. So $`e'`$ still can't be in any MST.

**Algorithm:** Build the small graph $`G'' = (V \cup \{v'\}, T \cup E')`$. This has at most $`2|V|`$ edges. Run Kruskal's on $`G''`$.

**Runtime:** $`O(|V| \log |V|)`$.

**Why this matters for Prox:** This is exactly what happens when a new product document arrives. The agent has an existing knowledge tree. The new document has connections to several existing nodes. The agent needs to figure out where to attach it and whether any existing connections should be reorganized.

---

## Edge Weight Change

**Problem:** You have an MST $`T`$ of graph $`G`$. The weight of edge $`e`$ changes. Find the new MST.

There are four cases. Two require work, two are no-ops.

### Case 1: Non-tree edge weight decreases

$`e \notin T`$ and $`w(e)`$ goes down. This edge might now be competitive.

**Algorithm:** Add $`e`$ to $`T`$, creating a cycle. Find the heaviest edge $`e^*`$ in the cycle. If $`w_{\text{new}}(e) < w(e^*)`$, swap: $`T' = T - e^* + e`$. Otherwise, no change.

This is identical to edge insertion.

### Case 2: Tree edge weight increases

$`e \in T`$ and $`w(e)`$ goes up. This edge might no longer belong.

**Algorithm:** Remove $`e`$ from $`T`$, splitting into components $`T_u`$ and $`T_v`$. Find the lightest non-tree edge $`e'`$ crossing the cut $`(T_u, T_v)`$. If $`w(e') < w_{\text{new}}(e)`$, swap: $`T' = T - e + e'`$. Otherwise, keep $`e`$ with its new weight.

This is the cut property in action.

### Case 3: Non-tree edge weight increases

$`e \notin T`$ and $`w(e)`$ goes up. An edge that already wasn't competitive gets worse. **No change.**

### Case 4: Tree edge weight decreases

$`e \in T`$ and $`w(e)`$ goes down. An edge that was already in the MST gets cheaper. **No change** (the tree structure stays the same, total weight decreases).

### The key insight

Cases 1 and 2 are duals:

- **Case 1 (non-tree decrease):** "Add edge, remove heaviest in cycle" -- **cycle property**
- **Case 2 (tree increase):** "Remove edge, add lightest across cut" -- **cut property**

This is the same duality from Part 1, applied to a graph that's changing.

**Why this matters for Prox:** This is what happens when a technician corrects information in the knowledge base. A relationship between two products was overstated (weight decrease on a non-tree edge -- maybe this connection is now important enough to restructure around). Or a relationship was understated (weight increase on a tree edge -- maybe the tree should route through a different path).

---

## Summary

| Change | What happens | Property used |
|---|---|---|
| New edge added | Add to tree, cycle forms, remove heaviest in cycle | Cycle property |
| New node added | Only MST edges + new edges matter, run Kruskal on small graph | Cycle property (to eliminate old non-tree edges) |
| Non-tree edge gets lighter | Same as edge insertion | Cycle property |
| Tree edge gets heavier | Remove edge, find lightest across the cut | Cut property |
| Non-tree edge gets heavier | Nothing | -- |
| Tree edge gets lighter | Nothing (total weight decreases) | -- |

## What to make sure you understand before the practice problems

- [ ] You can update an MST when a new edge arrives (add, find cycle, remove heaviest)
- [ ] You can explain why old non-tree edges can't enter the MST when a new node arrives
- [ ] You can identify which of the four weight-change cases applies and execute the update
- [ ] You see the duality: cycle property handles insertions/decreases, cut property handles deletions/increases

---

*Sources: MIT 6.1220/18.410J Recitation 5 (Fall 2023); Jeff Erickson, Algorithms, Ch. 7, Exercise 5.*
