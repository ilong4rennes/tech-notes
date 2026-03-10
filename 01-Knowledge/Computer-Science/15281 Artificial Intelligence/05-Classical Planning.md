# Classical Planning

## Learning Objectives

1. Compare and contrast classical planning methods with planning via search or propositional logic
2. Identify properties of a given planning algorithm, namely whether it is sound, complete, and optimal
3. Discuss how GraphPlan differs from searching in a state-space graph representation
4. Identify termination conditions from a GraphPlan planning graph
5. Implement and execute real-world planning problems using GraphPlan

## Classical Planning Representation

In classical planning, the framework extends beyond binary true/false values to include **objects** representing environmental entities. **Predicates** (or propositions) are boolean functions over these objects.

A **state** is a conjunction of predicates, as is the **goal**. **Operators** modify states through **preconditions** (predicates that must be true) and **effects** (predicates to add or delete).

Key distinction: "there is a difference between deleting a predicate and adding its negation."

### Example

In a blocks world scenario with blocks A, B, C and a manipulator hand, relevant predicates include:
- `In-Hand(C)`
- `On-Table(B)`
- `On-Block(A, B)`
- `Clear(A)`
- `Clear(C)`

---

## Planning Representations

### State-Space Graph

A **state-space graph** represents each state as a conjunction of predicates, with operators as edges. This approach enables standard graph search (BFS, DFS).

**Properties:**
- Sound: all solutions are legal plans
- Complete: solution found if one exists
- Optimal: BFS finds best path by operator count

**Limitation:** "the size is exponential in the number of predicates," with space complexity O(2^p) where p is the predicate count.

### GraphPlan Graph

**GraphPlan** uses a **planning graph** alternating between:
- **Proposition layers**: predicates potentially true after executing actions
- **Action layers**: applicable operators, including "no-op" operators for persistence

Edges represent preconditions (predicate to action) and effects (action to predicate).

**Mutex relations** (shown as grey lines) indicate mutually exclusive relationships between actions or propositions.

**Advantage:** More concise representation than state-space graphs.

**Heuristic property:** "GraphPlan is always correct if it determines that the goal is not reachable. If the goal is reachable, GraphPlan will never overestimate the number of steps."

---

## GraphPlan Algorithm

### Graph Expansion

Starting with proposition layer S₀ (initial state):

1. Add action layer A_t, connecting operators with preconditions in S_t
2. Add proposition layer S_{t+1}:
   - **Solid lines** to predicates the action adds
   - **Dotted lines** to predicates the action deletes
3. Include no-op operators persisting propositions

### Mutual Exclusion

Two actions are mutex if they exhibit:

1. **Interference:** one action's effect deletes/negates another's precondition
2. **Inconsistent effects:** actions negate each other's effects
3. **Competing needs:** preconditions are mutual negations

Two propositions are mutex if:
- They negate each other, or
- They have "inconsistent support" (no non-mutex action set produces both)

### GraphPlan Pseudocode

```
while True:
    Extend GraphPlan graph by adding action level then proposition level
    If no new propositions added from previous level:
        Return NO SOLUTION (graph levelled off)
    If all goal propositions present in added level:
        Search for possible plan in planning graph
        If plan found, return plan
```

### Searching for a Possible Plan

- Search states include propositions from a proposition layer plus a "goals" list
- Start at S_t (final planning graph level) with goal propositions
- Available actions: subsets of operators in A_{t-1} with no conflicts and effects covering all goals
- Transitions move to S_{t-1} with new goals being operator preconditions
- Success: reach S₀ where all goals are satisfied

---

## Using a GraphPlan Solver

### Core Components

**Instances:** Constants with Types (e.g., London and Paris as PLACE instances).

**Variables:** Can take any instance of matching type:
```
v_from = Variable('from', PLACE)
```

**Propositions:** Boolean descriptors functioning as state descriptors:
```
Proposition('at', i_package, i_london)
Proposition('fuel_at', i_ints[2])
```

**Start/Goal States:** Lists of propositions defining initial and target conditions.

### Operator Definition

```
o_move = Operator('move',
    # Preconditions
    [Proposition(NOT_EQUAL, v_from, v_to),
    Proposition('at', i_rocket, v_from),
    Proposition('fuel_at', v_fuel_start),
    Proposition(LESS_THAN, i_ints[0], v_fuel_start),
    Proposition(SUM, i_ints[1], v_fuel_end, v_fuel_start)],

    # Add effects
    [Proposition('at', i_rocket, v_to),
    Proposition('fuel_at', v_fuel_end)],

    # Delete effects
    [Proposition('at', i_rocket, v_from),
    Proposition('fuel_at', v_fuel_start)])
```

**Usage guideline:** Use instances for specific matches; use variables when any compatible instance suffices.

### Critical GraphPlan Consideration

"For any proposition named 'P', you can't just create another proposition named 'notP' and expect that GraphPlan can understand the relationship."

Implications:
- GraphPlan doesn't automatically infer mutual exclusivity
- Adding "notP" doesn't equal deleting "P"

This requires explicit design decisions in proposition structure.
