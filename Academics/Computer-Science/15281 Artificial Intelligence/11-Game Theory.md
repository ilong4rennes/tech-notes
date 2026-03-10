---
tags: [artificial-intelligence]
---

# Game Theory

## Learning Objectives

1. Formulate a problem as a game

2. Describe and compare the basic concepts in game theory
   - Normal-form game, extensive-form game
   - Zero-sum game, general-sum game
   - Pure strategy, mixed strategy, support, best response, dominance
   - Dominant strategy equilibrium, Nash equilibrium, Stackelberg equilibrium

3. Describe iterative removal algorithm

4. Compute equilibria for bimatrix games
   - Pure strategy Nash equilibrium
   - Mixed strategy Nash equilibrium
   - Stackelberg equilibrium

#### [Out of scope this semester] Social Choice

1. Understand the voting model
2. Find the winner under the following voting rules: Plurality, Borda count, Plurality with runoff, Single Transferable Vote
3. Describe concepts, axioms, and properties of voting rules
4. Understand the possibility of satisfying multiple properties
5. Describe the greedy algorithm for voting rule manipulation

## Basic Concepts

### Game Types

A game is "any set of circumstances that has a result dependent on the actions of two or more decision-makers."

**Normal Form:** Consists of players, possible actions for each player, and payoff/utility functions. A bimatrix game is a special case with 2 players and finite action sets. Players move simultaneously and the game ends immediately.

**Extensive Form:** Players move sequentially with a game tree structure, allowing representation of incomplete information.

**Zero Sum:** Utilities for players always sum to 0 or some constant regardless of actions chosen.

**General Sum:** The sum of player utilities is not constant and depends on actions.

### Strategies

**Pure Strategy:** Choose action deterministically

**Mixed Strategy:** Choose action according to a probability distribution, calculating expected utility by summing over all action profiles multiplied by their probabilities.

**Support:** Set of actions chosen with non-zero probability

### Dominance

For player i with strategies s_i and s_i':

**Strictly dominates:** u_i(s_i, s_j) > u_i(s_i', s_j) ∀ s_j

**Very weakly dominates:** u_i(s_i, s_j) ≥ u_i(s_i', s_j) ∀ s_j

**Weakly dominates:** u_i(s_i, s_j) ≥ u_i(s_i', s_j) ∀ s_j, and ∃ s_j where u_i(s_i, s_j) > u_i(s_i', s_j)

### Best Response

If s_i strictly dominates all other possible strategies, then s_i is a best response to any opponent strategy.

## Solution Strategies in Games

A solution concept is "a formal rule for predicting how a game will be played."

### Dominant Strategy Equilibrium

Achieved when every player plays a dominant strategy, such as both players defecting in prisoner's dilemma.

### Nash Equilibrium

"In a Nash Equilibrium, every player's strategy is a best response to the other players' strategy profile."

**Pure Strategy Nash Equilibrium (PSNE):** a_i ∈ BR(a_{-i}), ∀ i

**Mixed Strategy Nash Equilibrium (MSNE):** At least one player uses a randomized strategy; s_i ∈ BR(s_{-i}), ∀ i

A Nash Equilibrium always exists in finite games.

#### Finding a PSNE (Approach 1)

- Enumerate all action profiles
- For each profile, check if it is a Nash Equilibrium
- For each player, verify no beneficial deviation exists

#### Finding a PSNE through Iterative Removal (Approach 2)

"Strictly dominated strategies cannot be part of a Nash Equilibrium."

Process:
1. Remove strictly dominated actions (pure strategies)
2. Repeat until no actions can be removed
3. If one action remains per player: unique NE (dominance solvable)
4. Otherwise: find PSNE in remaining game
5. Order of removal does not matter

**Example:**

Starting game:
```
     L    C    R
U   3,1  2,2  1,3
M   1,3  4,4  2,2
D   2,2  1,1  1,4
```

Step 1: R is strictly dominated by C for player 2. Remove R.

Step 2: D is strictly dominated by M for player 1. Remove D.

Step 3: L is strictly dominated by C for player 2. Remove L.

Step 4: U is strictly dominated by M for player 1. Remove U.

Result: **PSNE = (M, C)** with payoff (4, 4).

#### Finding a MSNE

Apply iterative removal first, then solve for mixed strategies.

**Example (after iterative removal):**
```
       F    C
F     2,1  0,0
C     0,0  1,2
```

Let s_A = (p, 1-p) and s_B = (q, 1-q) with 0 < p, q < 1.

For player A: u_A(F, s_B) = u_A(C, s_B)
```
2q + 0(1-q) = 0q + 1(1-q)
2q = 1 - q
q = 1/3
```

For player B: u_B(s_A, F) = u_B(s_A, C)
```
1p + 0(1-p) = 0p + 2(1-p)
p = 2(1-p)
p = 2/3
```

Result: **MSNE** where s_A = (2/3, 1/3) and s_B = (1/3, 2/3)

### Minimax and Maximin Strategies

**Minimax Strategy:** Minimize the opponent's best-case expected utility (aim to harm opponent)

**Maximin Strategy:** Maximize worst-case expected utility

**Minimax Theorem:** In 2-player zero-sum games, Minimax = Maximin = NE; all NEs produce the same utility profile.

### Stackelberg Equilibrium

The leader commits to a strategy first; the follower responds after observing the leader's choice.

**Properties:**
- Follower best responds to leader strategy
- Leader commits to strategy maximizing leader's utility, assuming follower best responds
- In **Strong Stackelberg Equilibrium**, ties break in leader's favor
- Pure strategy can be found by enumerating leader's pure strategies
- Leader can commit to mixed strategy; u^SSE ≥ u^NE (first-mover advantage)

---

## Social Choice

Mathematical theory for "aggregation of individual preferences," primarily in voting contexts.

### Voting Model

Consists of:
- A set of voters {1,...,n}
- A set of candidates or alternatives A where |A| = m
- Each voter has a ranking of alternatives
- The **preference profile** is the collective ranking of all voters

### Voting Rules

#### Plurality

Each voter awards one point to their top alternative; the alternative with most points wins. Ties are possible.

#### Borda Count

With m alternatives, each voter assigns (m - k) points to the kth-ranked alternative. The alternative with most points wins.

#### Plurality with Runoff

The two top alternatives by plurality count advance to a pairwise election. Candidate x wins over y if the majority prefer x to y.

#### Single Transferable Vote (STV)

Used in Australia, New Zealand, and some US municipalities (San Francisco).

Process: m-1 rounds; each round eliminates the alternative with the lowest plurality score. Last remaining candidate wins.

### Social Choice Axioms

#### Majority Consistency

If an alternative gets more than 50% of the vote, it wins.

Borda Count is **not** majority consistent.

#### Condorcet Consistency

**Condorcet Winner:** The alternative that beats every other alternative in pairwise elections. May not exist when preferences cycle.

A voting rule is **Condorcet Consistent** if it selects the Condorcet winner when one exists.

Plurality and Borda Count are **not** Condorcet Consistent.

#### Strategy-Proofness

A voting rule is strategy-proof if a voter can never benefit from misrepresenting their preferences, no matter what others do.

Borda count and plurality (m ≥ 3) are **not** strategy-proof.

##### Greedy Algorithm for f-Manipulation

```python
def greedy_algorithm_f_manip(f, pref_profiles, y):
    # Input: f: voting rule
    #        pref_profiles: preference profiles of n - 1 voters
    #        y: alternative we want to make win uniquely
    # Output: final ranking of last voter that makes y win, or False

    final_ranking = init_empty_final_ranking()
    insert(final_ranking, y, rank=1)
    while unranked_alternatives:
        if exists_alt():
            x = get_alternative()
            insert(final_ranking, x)
        else:
            return False
    return final_ranking
```

#### Other Properties

**Dictatorial:** One voter always gets their most preferred alternative.

**Constant:** The same alternative is always chosen regardless of stated preferences.

**Onto:** Any alternative can win for some preference configuration.

### Gibbard-Satterthwaite Theorem

"If m ≥ 3, any voting rule that is strategy proof and onto is dictatorial."

Implications:
- Any onto and non-dictatorial voting rule is manipulatable
- Impossible to have a voting rule that is strategy-proof, onto, and non-dictatorial simultaneously
