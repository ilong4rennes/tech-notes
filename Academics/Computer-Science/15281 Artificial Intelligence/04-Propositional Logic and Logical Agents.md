---
tags: [artificial-intelligence]
---

# Propositional Logic and Logical Agents

## Learning Objectives

1. Define logic terms and apply them to real-world problems:
   - Model
   - Knowledge Base
   - Query
   - Entailment
   - Satisfiability

2. Define propositional logic terms:
   - Symbol
   - Literal
   - Operator
   - Sentence
   - Clause
   - Definite Clause
   - Horn Clause

3. Determine whether a knowledge base entails a query

4. Determine whether a given model satisfies a query

5. Compute whether a sentence is satisfiable

6. Describe model checking and theorem proving algorithms (forward chaining and resolution), including runtime and space requirements

7. Define soundness and completeness for model checking and theorem proving algorithms

8. Describe Boolean satisfiability problems (SAT)

9. Describe conjunctive normal form (CNF) and its necessity for certain algorithms

10. Implement successor-state axioms to encode logic for how fluents change over time

11. Describe and implement SAT Plan (planning as satisfiability) to real-world problems

## Logical Agents

If we create variables representing statements to verify and variables for starting conditions, we can search all variable assignments to check if goal conditions are true. Alternatively, we can use logical statements to infer new variable values from starting conditions (inference).

**Main Idea**: "The agent encodes percepts that it receives as logical statements that are added to a knowledge base of logical rules and facts. The agent then reasons about what to do based on the knowledge base."

### Vocabulary

- **Model:** A complete assignment of symbols to True or False

- **Sentence:** Logical statement composed of logical symbols and operators

- **Knowledge Base:** Collection of sentences representing what we know about the world. Sentences are effectively all AND-ed together.

- **Query:** Sentence we want to know is definitely true, definitely false, or unknown

## Propositional Logic

### Symbols and Operators

- **Symbols:** Variables that can be true or false (usually represented by capital letters).

- **Operators:**
  - ¬A: Not A (Negation)
  - A ∧ B: A and B (Conjunction)
  - A ∨ B: A or B (Disjunction) — inclusive rather than exclusive or
  - A ⇒ B: A implies B (Implication)
  - A ⇔ B: A if and only if B (Biconditional)

### Sentence Definition

Defined constructively as:

**Base Case:**
- A symbol (e.g. X) is a sentence

**Recursive Cases:**
- If A is a sentence, ¬A is a sentence
- If A, B are sentences, A ∧ B is a sentence
- If A, B are sentences, A ∨ B is a sentence
- If A, B are sentences, A ⇒ B is a sentence
- If A, B are sentences, A ⇔ B is a sentence

### Literal

An atomic sentence. True, False, A, ¬A (where A is a symbol) are literals.

### Clause

A disjunction of literals. Two example clauses:
- α₁: A ∨ ¬B ∨ ¬C
- α₂: A ∨ B ∨ ¬C

**Definite Clause:** A clause with exactly one positive (not negated) literal. α₁ is a definite clause while α₂ is not.

**Horn Clause:** A clause with at most one positive literal. α₁ is a horn clause and α₂ is not.

### Conjunctive Normal Form (CNF)

A conjunction of one or more clauses. For instance, α₁ ∧ α₂ is a sentence in CNF. One way to think of this is an AND of ORs.

### Converting Propositional Logic to CNF

Useful rules for converting sentences into CNF:

- α ⇔ β ≡ (α ⇒ β) ∧ (β ⇒ α) [Biconditional Elimination]

- α ⇒ β ≡ ¬α ∨ β [Implication Elimination]

- ¬(α ∧ β) ≡ ¬α ∨ ¬β [DeMorgan's Law]

- α ∨ (β ∧ γ) ≡ (α ∨ β) ∧ (α ∨ γ) [Distribute ∨ over ∧]

## Entailment

**Entailment (α ⊨ β):** "α entails β iff for every model that causes α to be true, β is also true. In other words, the α-models (models that cause α to be true) are a subset of the β-models."

### Properties

- **Sound:** Everything it claims to prove is entailed

- **Complete:** Every sentence that is entailed can be proved

Usually, we want to know whether the knowledge base entails a specific query. Two main methods are used: model checking and theorem proving.

### Model Checking

(Simple) Model checking enumerates all possible models and checks that for every model making the KB sentence true, the query is also true. This uses truth tables to prove entailment.

As anyone constructing a truth table with ≥3 variables knows, this is reliable (sound and complete) yet wildly inefficient, running in exponential time with respect to the number of symbols.

### Theorem Proving

Two key inference rules used in theorem proving algorithms:

- **Modus Ponens:** Given X₁ ∧ X₂ ∧ ... ∧ Xₙ ⇒ Y and X₁ ∧ X₂ ∧ ... ∧ Xₙ, infer Y.

- **General Resolution:** Given A₁ ∨ A₂ ∨ ¬B and B ∨ C₁ ∨ C₂, infer A₁ ∨ A₂ ∨ C₁ ∨ C₂ (law of excluded middle — we know B must be either True or False, so if both sentences are true, either (A₁ ∨ A₂) or (C₁ ∨ C₂) must be true).

Resolution and forward chaining are two algorithms used for theorem proving. The resolution algorithm uses the resolution inference rule, and forward chaining uses Modus Ponens.

#### Resolution Algorithm

KB ⊨ α means for all models making KB true, α is also true. This is equivalent to saying there does not exist any model such that KB ∧ ¬α is true. We can prove KB ∧ ¬α is unsatisfiable through proof by contradiction.

We start with CNF clauses, including clauses in the KB and ¬α. Then, we repeatedly use general resolution to resolve pairs of clauses until resolving an empty clause, i.e., E ∧ ¬E ⇒ False. When that happens, KB ∧ ¬α is unsatisfiable. Therefore, KB ⊨ α. If we run out of possible pairs of clauses to resolve without resolving an empty clause, then KB does not entail α.

The resolution algorithm is sound and complete for any propositional logic KB, but its runtime complexity is exponential with respect to the number of symbols. The PL KB must be converted to CNF first, but any PL sentence can be converted to CNF.

##### Example

KB = P, ¬P ∨ Q, P ∨ ¬Q ∨ R, ¬Q ∨ R

From the given Knowledge Base, prove R to be true.

#### Forward Chaining

Forward chaining is similar in structure to graph search algorithms. Instead of taking actions, we apply Modus Ponens to generate symbols known to be true. We "find the goal" if we can conclude that the query symbol is true.

Forward chaining has a significant limitation: the knowledge base must consist of only definite clauses, and the query must be just a positive literal. Definite clauses can be written in the form: (conjunction of positive literals) ⇒ a positive literal.

### Algorithm Steps

1. For all clauses in KB and query, count the number of symbols in the premise. For example, if P ∧ Q ⇒ L, count = 2.

2. Add symbols initially known to be true in KB to an **agenda.** The agenda contains symbols known to be true but not yet used to infer other symbols. It's a queue, similar to the frontier in graph search.

3. Initialize a dictionary called **inferred**, where inferred[s] is false for all symbols. This avoids adding already inferred symbols to the agenda again (like an explored set).

4. While agenda is not empty, pop symbol P off the agenda. If P is the query symbol, KB entails the query. Otherwise, if inferred[P] is false, for all clauses having P in the premise, decrease the corresponding count. If count is 0, by Modus Ponens, we can infer the conclusion since all premises are true. Add this new inferred symbol to the agenda and set inferred[P] to true.

5. Repeat step 4 until:
   - We've inferred the query (return true), or
   - We've exhausted the agenda (return false).

This algorithm is sound and complete for definite clause knowledge bases and runs in linear time. A definite clause is a clause with exactly one positive literal, such as (¬A ∨ B ∨ ¬C), equivalent to an implication sentence like A ∧ C ⇒ B.

##### Example

- KB: [A, B, A ∧ B ⇒ L, B ∧ L ⇒ P, P ⇒ Q]
- Query: Q

Initially, count = (0, 0, 2, 2, 1) for each sentence in KB as ordered above, and agenda will have A, B.

1. Pop A. Now inferred[A] is True. Count for clause becomes: (0, 0, **1**, 2, 1).

2. Pop B. inferred[B] is True. Count for clauses becomes: (0, 0, **0**, **1**, 1). Notice the count for clause A ∧ B ⇒ L became 0. Hence, by Modus Ponens we can infer L. Add L to the agenda.

3. Pop L. inferred[L] is True. Count for clauses becomes: (0, 0, 0, **0**, 1). Add P to the agenda.

4. Pop P. inferred[P] is True. Count for clauses becomes: (0, 0, 0, 0, **0**). Add Q to the agenda.

5. Q = query, so KB ⊨ Q!

## Satisfiability

**Satisfiability:** "We say that a sentence α is satisfiable if there exists at least one model that causes the sentence to evaluate to true."

- We say that the model **satisfies** the sentence.
- As a trivial example, the model A = True, B = False satisfies the sentence A ∧ ¬B
- The sentence A ∧ ¬B is satisfiable because it is true with model A = True, B = False. We just need to find one model to show satisfiability.

### Check Satisfy

It is natural to need a way to check if a given sentence is satisfied by a particular model. The PL-True algorithm does exactly this. It takes in a sentence α along with the model and evaluates as follows:

1. If α is a literal, then just return the value of that literal from the model.

2. If α contains a logical operator:
   - If the operator is ¬, then recursively call the function with the argument to the negation, and then negate the return value.
   - If the operator is ∧, recursively call the function on both arguments to the ∧ and then ∧ the two results together.
   - This goes on similarly for all other logical operators.

### Boolean Satisfiability Problem

This problem refers to trying to see if a given sentence is satisfiable or not. A **SAT-solver** is an algorithm that can do this.

#### SAT with Model Checking

One simple SAT-solver would enumerate every model (all combinations of assigning True and False to every symbol), but this costs O(2ⁿ) if there are n symbols and may not be feasible for larger problems.

One big difference between using model checking for SAT versus model checking for entailment: with SAT, we return early (with that model as the result) as soon as we find one model making the sentence true, whereas with entailment, we can return early (with result False) if we find one model making KB true but **not** the query.

#### SAT with Resolution

The bulk of the resolution algorithm for proving entailment is actually solving the SAT problem. Specifically, the resolution entails algorithm returns true if ¬SAT(KB ∧ ¬query). When the resolution algorithm finds an empty clause, the input KB ∧ ¬query is not satisfiable. If the resolution algorithm runs out of possible clauses to resolve without finding the empty clause, then KB ∧ ¬query is satisfiable, but we would still have work to do if we wanted to find a specific model.

#### DPLL Algorithm

This algorithm is essentially CSP backtracking search with heuristics and checks to ensure minimum work. It only works for sentences in conjunctive normal form.

DPLL initially checks for three things:

1. **Early Termination:** All clauses are satisfied (True) or any are unsatisfied (False)
   - DPLL can stop. If all are satisfied then the sentence is satisfiable, and if one is not, then the sentence is not satisfiable, so we return false (the input is in CNF).

2. **Pure Literals:** If all occurrences of any variable have the same sign (all negated or all not negated), assign a value making it true
   - This smartly assigns values to variables so the least number of possible models will be enumerated.

3. **Unit Clauses:** If one of the clauses in the sentence is just a single variable, assign the value making the clause true.

### Full DPLL Algorithm

1. Check early termination and return true or false as described above.

2. If the algorithm cannot terminate early, find a pure literal then assign that to the correct value and recurse with your new model where that literal is now assigned a value.

3. If there are no pure literals, find a unit clause and repeat the above step.

4. If DPLL cannot do any of the above steps, choose any variable not yet assigned and recurse twice: one case assigning the literal to True and the other to False. Return the logical or of these two cases.

In the worst case this will still expand every possible model, but in some cases the pure literal and unit clause heuristics will save computation.

##### Example

Determine whether the following sentence is satisfiable or unsatisfiable using DPLL. Break ties by assigning variables in alphabetical order, starting with True. If satisfiable, what model does the algorithm find?

(A ∨ B ∨ ¬E) ∧ (B ∨ ¬C ∨ D) ∧ (¬A ∨ ¬B) ∧ (¬A ∨ ¬C ∨ ¬D) ∧ E

Assign True to E (unit clause): (A ∨ B) ∧ (B ∨ ¬C ∨ D) ∧ (¬A ∨ ¬B) ∧ (¬A ∨ ¬C ∨ ¬D)

Assign False to C (pure symbol): (A ∨ B) ∧ (¬A ∨ ¬B)

Assign True to A (value assignment): ¬B

Assign False to B (unit clause or pure symbol): This sentence is satisfiable and we assign D to be true arbitrarily since it was not assigned during the algorithm; the model found is A: True, B: False, C: False, D: True, E: True.

## Logical Agents (Time and Planning)

### Fluents

A key first step to planning with propositional logic is figuring out how to deal with things changing over time. For example, in Wumpus World, the agent has an arrow that it can use to slay the Wumpus. We're tempted to change the value of symbols like HaveArrow and WumpusAlive from True to False, but this is not how propositional logic works.

Symbols that may change their value over time are called **fluents.** We deal with the passage of time for a fluent by creating a different symbol for each possible time point. We can incorporate the time index as part of the symbol name. For time points 0-3 we would have symbols:

HaveArrow[0], HaveArrow[1], HaveArrow[2], HaveArrow[3]

For planning problems, our actions will have time indices to indicate whether or not we took that action at that time index:

Shoot[0], Shoot[1], Shoot[2], Shoot[3]

### Effect Axioms

To encode the logic of how fluents change over time, we could consider using what are called **effect axioms.** Effect axioms might be our first approach to intuitively write logic cause and effect statements, however, as we'll see, they can often lead to unexpected flaws.

For example, we may write the following to show the cause and effect of shooting an arrow at time t:

HaveArrow[t] ∧ Shoot[t] ⇒ ¬HaveArrow[t+1]

But what if the left-hand side of this implication is false, for example, when Shoot[t] is false? This would still allow for ¬HaveArrow[t+1] to be true (remember F ⇒ T is always true). We can tighten this up with a biconditional to say that we lose an arrow if and only if we have an arrow and shoot:

HaveArrow[t] ∧ Shoot[t] ⇔ ¬HaveArrow[t+1]

This is better but starts to break if there is more than one way for the agent to no longer have an arrow. For example, if there were a SellArrow[t] action in the game, the above logic would prohibit the act of selling an arrow at time t to result in ¬HaveArrow[t+1].

We're going to fix some of these issues with a more rigorous formula for handling fluents called successor state axioms.

### Successor State Axioms

**Successor-state axioms** are axioms outlining a rigorous way of writing the logic of how a fluent changes over time. These axioms state that a fluent can be true at time t+1 if and only if:

1. There was some action at time t that made it true, or

2. It was already true at time t and no action occurred that would make it false

The general schema for successor state axioms for fluent F is:

F[t+1] ⇔ (¬F[t] ∧ ActionCausesF[t]) ∨ (F[t] ∧ ¬ActionCausesNotF[t])

For example, consider a game with actions Shoot, SellArrow, and BuyArrow (max one arrow per agent). The successor state axiom for the fluent HaveArrow at time t+1 would be:

HaveArrow[t+1] ⇔ (¬HaveArrow[t] ∧ BuyArrow[t]) ∨ (HaveArrow[t] ∧ ¬(Shoot[t] ∨ SellArrow[t]))

### Planning as Satisfiability

If we have a very efficient SAT-solver, then we can make a plan. Note that the environment must be fully observable and deterministic.

If we want to find a plan that takes as few time steps as possible, we can run an algorithm called SATPlan that increases the max allowable time one step at a time until it can find a satisfiable solution in that number of time steps.

#### SATPlan Algorithm

1. Let maxT = 1

2. Build a KB with all of the logic for time points 0 to maxT. This includes:
   - The initial state (at time zero)
   - Successor state axioms for any fluents for time points 1 to maxT
   - Any other logic of the environment, for example, exactly one action per time point

3. Formulate the goal at time maxT

4. Use the SAT-solver to see if there is a model that satisfies KB and goals.
   - If yes, extract the sequence of actions from that model
   - If no, increment maxT by 1 and try again, repeating steps 2-4

Rather than looping forever, practically, we can set a limit on how high we let maxT get.
