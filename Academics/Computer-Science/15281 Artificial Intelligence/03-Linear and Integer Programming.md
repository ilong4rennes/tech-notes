# Linear and Integer Programming

## Learning Objectives

1. Formulate a problem as a Linear Program (LP)
2. Convert an LP into a required form, e.g., inequality form
3. Plot the graphical representation of a linear optimization problem with two variables and find the optimal solution
4. Understand the relationship between optimal solution of an LP and the intersections of constraints
5. Describe and implement an LP solver based on vertex enumeration
6. Describe the high-level idea of the Simplex (hill climbing) algorithm
7. Formulate a problem as an Integer or Linear Program (IP or LP)
8. Write down the Linear Program (LP) relaxation of an IP
9. Plot the graphical representation of an IP and find the optimal solution
10. Understand the relationship between optimal solution of an IP and the optimal solution of the relaxed LP
11. Describe and implement branch-and-bound algorithm
12. Understand ethical concerns around the high resource cost needed to solve many modern computing problems

## Introduction and Formulation

Optimization problems relate closely to constraint satisfaction problems. The primary distinction involves introducing a notion of **cost** to minimize or maximize, whereas CSPs focus on any variable assignment satisfying constraints.

The first step involves defining variables used to formulate the **cost vector** (objective function) and **constraints**.

The general LP formulation format is:

$$\min_{\mathbf{x}} c^T\mathbf{x}$$
$$\text{s.t. } \mathbf{Ax} \preceq \mathbf{b}$$

Where:
- $\mathbf{c}$ = the cost vector
- $\mathbf{x}$ = variable vector
- $\mathbf{A}$, $\mathbf{b}$ = coefficient matrix and vector representing constraints

Any constraint not in $\preceq$ form can be altered to match the standard format.

### Cost Contours

The LP cost function $f(x) = c^Tx$ defines cost contours — "the set of all points $x'$ such that $k = f(x')$ for a given $k$" (equivalent to level curves in calculus).

For example, with $c = [1\ 3]^T$, the function $z = x + 3y$ creates a plane. Cost contours emerge from cross-sections at varying z-values.

**Key properties:**
- Cost **increases** traveling in the cost vector direction
- Cost **decreases** in the opposite direction
- Contour lines are **perpendicular** to the cost vector direction
- Increasing cost vector magnitude makes contours closer together

## Algorithms for Solving Linear Programs

If a linear program has a solution, it will be located at one of the feasible intersections of constraint boundaries. Unbounded feasible regions may yield negative infinity solutions.

Two main algorithms:

1. **Vertex Enumeration:** Evaluate the objective function at all feasible intersections and select the one with the smallest value (for minimization problems)
2. **Simplex:** Start at an arbitrary intersection, iteratively move to the best neighboring vertex until no improvement is possible

### Example

Consider the objective function $2x_1 + 3x_2$ with constraints:

$$-x_2 \leq -1$$
$$x_1 + 3x_2 \leq 11$$
$$-2x_1 + x_2 \leq -1$$
$$2x_1 + x_2 \leq 12$$

**Vertex Enumeration approach:**

Step 1: Evaluate objective function at all feasible intersections
- Point A: $2(1) + 3(1) = 5$
- Point B: $13$
- Point C: $16$
- Point D: $14$

Step 2: Select the vertex with smallest value. Point A yields the optimal solution: $x_1 = 1, x_2 = 1$.

**Simplex approach:**

Step 1: Choose arbitrary vertex, e.g., $C = 16$
Step 2: Evaluate neighboring vertices ($B = 10, D = 14$), move to best neighbor (B)

(Continue the algorithm following this pattern)

## Algorithm for Solving Integer Programs: Branch and Bound

When all variables must have **integer** values, we need a different solution approach. The **Branch and Bound algorithm** addresses this constraint.

### Example

Consider the Integer Programming problem:

$$\min_x -3x_1 - 5x_2$$
$$\text{s.t. } 2x_1 + 4x_2 \leq 25$$
$$x_1 \leq 8$$
$$2x_2 \leq 10$$
$$x_1, x_2 \geq 0$$
$$x_1, x_2 \in \mathbb{Z}$$

**Process:**

At the root node, solve the relaxed LP problem (removing integer constraints). The solution is point A: $(8, 2.25)$ with objective value $-35.25$.

Since $(8, 2.25)$ is not integer-valued, **branch** by adding constraints:
- $x_2 \leq 2$
- $x_2 \geq 3$

This creates two subproblems with costs $-34$ and $-34.5$ respectively. Explore the $-34.5$ subproblem first.

Subproblem 2 yields $(6.5, 3)$ with cost $-34.5$ — still non-integer, requiring further branching.

The final search tree exploration (in priority queue order) yields the IP solution: $(8, 2)$ with cost $-34$.

**Questions for practice:**
- For each tree leaf, identify why the branch bounded (preventing further search)
- Could a different priority order explore the optimal IP solution faster?
