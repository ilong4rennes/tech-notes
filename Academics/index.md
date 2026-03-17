---
tags: [index]
---

## Contents

- [Computer Science](#computer-science)
	- [10301 Machine Learning](#10301-machine-learning)
	- [15213 Computer Systems](#15213-computer-systems)
	- [15281 Artificial Intelligence](#15281-artificial-intelligence)
	- [17313 Software Engineering](#17313-software-engineering)
- [Information Systems](#information-systems)
	- [67250 Information Systems Milieux](#67250-information-systems-milieux)
	- [67265 Design Fundamentals](#67265-design-fundamentals)
	- [67272 Application Design and Development](#67272-application-design-and-development)
	- [67364 Practical Data Science](#67364-practical-data-science)
	- [67373 Consulting Project](#67373-consulting-project)
- [Philosophy & Psychology](#philosophy-psychology)
	- [80100 Intro to Philosophy](#80100-intro-to-philosophy)
	- [80226 Philosophy of Science](#80226-philosophy-of-science)
	- [80261 Epistemology](#80261-epistemology)
	- [80335 Social and Political Philosophy](#80335-social-and-political-philosophy)
	- [85413 Perception](#85413-perception)

---

## Computer Science

### 10301 Machine Learning

| Name                     | Link                            | Summary                                                                             |
| ------------------------ | ------------------------------- | ----------------------------------------------------------------------------------- |
| Background               | [[00 Background]]               | Prerequisites: math and stats foundations underlying ML                             |
| Intro                    | [[01 Intro]]                    | ML problem formulation (Task, Performance, Experience); supervised learning routine |
| Decision Trees           | [[02 Decision Trees]]           | Tree-based classifiers; splitting criteria; overfitting                             |
| KNN                      | [[03 KNN]]                      | K-Nearest Neighbors; distance metrics; choosing k                                   |
| Perceptron               | [[04 Perceptron]]               | Linear classifier; weight updates; convergence                                      |
| Linear Regression        | [[05 Linear Regression]]        | Least squares; closed-form solution; gradient descent                               |
| Logistic Regression      | [[06 Logistic Regression]]      | Probabilistic classification; sigmoid; MLE                                          |
| Model Selection          | [[07 Model Selection]]          | Cross-validation; bias-variance tradeoff; hyperparameter tuning                     |
| Regularization           | [[08 Regularization]]           | L1/L2 penalties; ridge and lasso; preventing overfitting                            |
| Neural Networks          | [[09 Neural Networks]]          | Feedforward networks; backpropagation; activation functions                         |
| Learning Theory          | [[10 Learning Theory]]          | PAC learning; sample complexity; generalization bounds                              |
| Societal Impacts         | [[11 Societal Impacts]]         | Fairness, bias, ethics, and societal implications of ML                             |
| Deep Learning            | [[12 Deep Learning]]            | CNNs, RNNs, deep architectures, training techniques                                 |
| Reinforcement Learning   | [[13 Reinforcement Learning]]   | Rewards, policies, Q-learning from an ML perspective                                |
| Dimensionality Reduction | [[14 Dimensionality Reduction]] | PCA; feature compression; variance preservation                                     |
| K-Means                  | [[15 K-Means]]                  | Unsupervised clustering; centroid updates; convergence                              |
| Ensemble Methods         | [[16 Ensemble Methods]]         | Bagging, boosting, random forests; combining weak learners                          |
| Recommender Systems      | [[17 Recommender Systems]]      | Collaborative filtering; matrix factorization; user-item models                     |

---

### 15213 Computer Systems

| Name                       | Link                               | Summary                                                            |
| -------------------------- | ---------------------------------- | ------------------------------------------------------------------ |
| Course Overview            | [[01 Course Overview]]             | Intro to systems thinking; why low-level knowledge matters         |
| Computer Arithmetic        | [[02 Computer Arithmetic]]         | Integer vs float representation; overflow; bit manipulation        |
| Machine Level Programming  | [[03 Machine Level Programming]]   | x86-64 assembly; registers; control flow; stack frames             |
| Linking                    | [[04 Linking]]                     | Static and dynamic linking; symbol resolution; relocation          |
| Code Optimization          | [[05 Code Optimization]]           | Compiler optimizations; loop unrolling; memory access patterns     |
| Memory Hierarchy           | [[06 Memory Hierarchy]]            | Registers, cache, RAM, disk; locality principles                   |
| Cache Memories             | [[07 Cache Memories]]              | Cache organization; hit/miss; direct-mapped vs set-associative     |
| Virtual Memory             | [[08 Virtual Memory]]              | Address spaces; page tables; TLB; memory protection                |
| Dynamic Memory Allocation  | [[09 Dynamic Memory Allocation]]   | malloc/free; heap management; fragmentation                        |
| Processes and Multitasking | [[10 Processes and Multitasking]]  | Process model; context switching; fork and exec                    |
| Exceptional Control Flow   | [[11 Exceptional Control Flow]]    | Interrupts; signals; exceptions; system calls                      |
| System-Level IO            | [[12 System-Level IO]]             | Unix I/O; file descriptors; buffering                              |
| Network Programming        | [[13 Network Programming]]         | Sockets; TCP/IP; client-server model                               |
| Concurrent Programming     | [[14 Concurrent Programming]]      | Threads; synchronization; deadlock; semaphores                     |
| Chinese Reference          | [[chi]]                            | Chinese-language summary of course concepts                        |
| English Reference          | [[eng]]                            | English-language reference summary                                 |

---

### 15281 Artificial Intelligence

| Name                                   | Link                                            | Summary                                                                                          |
| -------------------------------------- | ----------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Search                                 | [[01-Search]]                                   | Uninformed and informed search; BFS, DFS, A*; heuristics                                         |
| Constraint Satisfaction Problems       | [[02-Constraint Satisfaction Problems]]         | CSP formulation; backtracking; arc consistency; MRV heuristic                                    |
| Linear and Integer Programming         | [[03-Linear and Integer Programming]]           | LP formulation; vertex enumeration; Simplex; branch and bound for IPs                            |
| Propositional Logic and Logical Agents | [[04-Propositional Logic and Logical Agents]]   | KB, entailment, CNF, resolution, forward chaining, DPLL, SATPlan, fluents                        |
| Classical Planning                     | [[05-Classical Planning]]                       | State-space graphs; GraphPlan; mutex relations; planning graph expansion                          |
| Markov Decision Process                | [[06-Markov Decision Process]]                  | MDP formulation; Bellman equations; value iteration; policy iteration                             |
| Reinforcement Learning                 | [[07-Reinforcement Learning]]                   | TD learning; Q-learning; ε-greedy; approximate Q-learning with feature weights                   |
| Probability                            | [[08-Probability]]                              | Conditional probability; Bayes' theorem; chain rule; marginalization; independence               |
| Bayes Nets                             | [[09-Bayes Nets]]                               | Graphical models; d-separation; variable elimination; prior/rejection/LW/Gibbs sampling           |
| HMMs and Particle Filters              | [[10-HMMs and Particle Filters]]                | Hidden Markov Models; forward-backward algorithm; particle filtering                             |
| Game Theory                            | [[11-Game Theory]]                              | Normal/extensive form; Nash equilibrium; MSNE; Stackelberg; social choice                        |

---

### 17313 Software Engineering

| Name                                      | Link                                              | Summary                                                                    |
| ----------------------------------------- | ------------------------------------------------- | -------------------------------------------------------------------------- |
| Introduction                              | [[01 Introduction]]                               | Software engineering failures (Vasa, CrowdStrike); requirements; QA; DevOps |
| Software Archaeology                      | [[02 Software Archaeology]]                       | Reading and understanding legacy codebases                                 |
| Case Study                                | [[03 Case Study]]                                 | Real-world SE case analysis                                                |
| Metrics and Measurement                   | [[04 Metrics and Measurement]]                    | Code quality metrics; measuring complexity and coverage                    |
| Process: Milestones, Estimation, Planning | [[05 Process -  Milestones, Estimation, Planning]] | Software process models; effort estimation; project planning               |
| Software Teams and Communication          | [[06 Software Teams and Communication]]           | Team structures; communication patterns; coordination                     |
| Design Docs                               | [[07 Design Docs]]                                | Writing technical design documents; RFCs; spec structure                   |
| Integrating AI into Products              | [[08 Integrating AI into Products]]               | Engineering challenges when shipping AI/ML features                        |
| Intro to Software Architecture            | [[09 Intro to Software Architecture]]             | Architectural styles; quality attributes; design decisions                 |
| Architecture: Modularity & Microservices  | [[10 Architecture - Modularity & Microservices]]  | Modular design; microservices tradeoffs; service decomposition             |
| Code Review Risk                          | [[11 Code Review Risk]]                           | Risk-based code review; prioritizing reviews; defect prediction            |
| Software Analysis Tools                   | [[12 Software Analysis Tools]]                    | Static analysis; linters; formal verification tools                        |
| AIML, Dynamic Analysis, OSS, Dependencies | [[14-21 AIML, Dynamic Analysis, OSS, Dependencies]] | AI/ML in SE; dynamic analysis; open source; dependency management        |

---

## Information Systems

### 67250 Information Systems Milieux

| Name                                                | Link                                                        | Summary                                                                    |
| --------------------------------------------------- | ----------------------------------------------------------- | -------------------------------------------------------------------------- |
| Information Systems in the Digital Age              | [[01 Information Systems in the Digital Age]]               | What IS is; types of systems (POS, ERP, etc.); sociotechnical effects      |
| The Enterprise Strategy and Business Process Layers | [[02 The Enterprise Strategy and Business Process Layers]]  | Business strategy alignment with IS; process modeling and workflows        |
| The Enterprise Application Layer                    | [[03 The Enterprise Application Layer]]                     | Enterprise applications (ERP, CRM, SCM); software integration              |
| The Enterprise Information and Infrastructure Layers | [[04 The Enterprise Information and Infrastructure Layers]] | Data management; cloud; infrastructure supporting enterprise IS            |
| Information Systems in the Enterprise and Beyond    | [[05 Information Systems in the Enterprise and Beyond]]     | IS at organizational and societal scale; digital transformation            |

---

### 67265 Design Fundamentals

| Name               | Link                        | Summary                                                                   |
| ------------------ | --------------------------- | ------------------------------------------------------------------------- |
| Design Fundamentals | [[67265 Design Fundamentals]] | Core design principles: visual hierarchy, typography, layout, UX fundamentals |

---

### 67272 Application Design and Development

| Name                            | Link                                      | Summary                                                           |
| ------------------------------- | ----------------------------------------- | ----------------------------------------------------------------- |
| Getting Started                 | [[00 Getting Started]]                    | Rails setup; git workflow; generating models and scaffolding      |
| Use Cases                       | [[01 Use Cases]]                          | Use case modeling; actors; requirements documentation             |
| MVC Pattern                     | [[02 MVC Pattern]]                        | Model-View-Controller architecture; separation of concerns        |
| Ruby                            | [[03 Ruby]]                               | Ruby syntax; data types; OOP; blocks and iterators                |
| Rails                           | [[04 Rails]]                              | Rails conventions; routing; migrations; ActiveRecord basics       |
| Relationships and Scopes        | [[05 Relationships and Scopes]]           | has_many, belongs_to, has_many :through; named scopes             |
| Testing Relationships and Scopes | [[06 Testing Relationships and Scopes]]  | RSpec/Minitest for model associations and scopes                  |
| Validations and Testing         | [[07 Validations and Testing]]            | ActiveRecord validations; testing valid/invalid states            |
| Callbacks and Testing           | [[08 Callbacks and Testing]]              | before/after callbacks; testing side effects                      |
| Views and Controllers           | [[09 Views and Controllers]]              | ERB templates; controller actions; params; redirects              |
| Regular Expressions             | [[10 Regular Expressions]]               | Regex syntax; matching and capturing; use in validations          |
| Authentication                  | [[11 Authentication]]                    | User sessions; password hashing; devise or custom auth            |
| Authorization                   | [[12 Authorization]]                     | Role-based access control; cancancan; restricting actions         |
| Ruby Object Model               | [[13 Ruby Object Model]]                 | Modules; mixins; method lookup chain; metaprogramming basics      |
| Refactoring                     | [[14 Refactoring]]                       | Code smells; extract method; DRY principles in Rails              |
| API                             | [[15 API]]                               | Building RESTful APIs in Rails; JSON responses; versioning        |
| Adding Search                   | [[16 Adding Search]]                     | Search implementation; filtering; Ransack or custom queries       |
| Testing Controllers and Views   | [[17 Testing Controllers and Views]]     | Integration and controller tests; view testing                    |
| React                           | [[18 React]]                             | Integrating React with Rails; components; props and state         |
| Design                          | [[19 Design]]                            | UI/UX design in the context of Rails apps                         |
| Data Visualization and Quality  | [[20 Data Visualization and Data Quality]] | Charting libraries; data cleaning; quality assurance            |
| Deployment                      | [[21 Deployment]]                        | Deploying Rails to Heroku or similar; environment config          |
| Phase 2                         | [[Phase 2]]                              | Project phase 2 notes and requirements                            |
| Phase 3                         | [[Phase 3]]                              | Project phase 3 notes and requirements                            |
| Questions                       | [[Questions]]                            | Running list of questions from coursework                         |
| Solved                          | [[Solved]]                               | Reference of solved problems and debugging notes                  |
| Review                          | [[review]]                               | Exam or quiz review material                                      |

---

### 67364 Practical Data Science

| Name                       | Link                                | Summary                                                              |
| -------------------------- | ----------------------------------- | -------------------------------------------------------------------- |
| Introduction               | [[00 Introduction]]                 | Course overview; data science workflow; tools                        |
| Exploratory Data Analysis  | [[01 Exploratory Data Analysis]]    | Summary statistics; distributions; visualizations; detecting anomalies |
| Machine Learning           | [[02 Machine Learning]]             | Applied ML pipelines; feature engineering; model evaluation          |
| Big Data                   | [[03 Big Data]]                     | Distributed computing; Spark; handling large-scale datasets          |

---

### 67373 Consulting Project

| Name                              | Link                                       | Summary                                                                        |
| --------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------ |
| Consulting Models                 | [[01 Consulting Models]]                   | Frameworks for structuring consulting engagements; MECE; issue trees           |
| Client Context & Problem Analysis | [[02 Client Context & Problem Analysis]]   | Scoping a client problem; stakeholder analysis; hypothesis-driven approach     |

---

## Philosophy & Psychology

### 80100 Intro to Philosophy

| Name        | Link           | Summary                                                                   |
| ----------- | -------------- | ------------------------------------------------------------------------- |
| Linguistics | [[Linguistics]] | Philosophy of language; meaning, reference, and how words relate to the world |
| Metaphysics | [[Metaphysics]] | Nature of reality and existence; ontology; identity and persistence       |

---

### 80226 Philosophy of Science

| Name                                  | Link                                          | Summary                                                                                                                  |
| ------------------------------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Kuhn — Scientific Revolutions         | [[01-Kuhn-Scientific-Revolutions]]            | Paradigm, normal science, anomalies, crisis, revolution; incommensurability; theory-ladenness                            |
| Greek Astronomy & Aristotle           | [[02-Greek-Astronomy-and-Aristotle]]          | Saving the appearances; instrumentalism vs realism; teleology vs mechanism; four causes                                  |
| Ptolemy & Copernicus                  | [[03-Ptolemy-and-Copernicus]]                 | Mathematical models vs reality; heliocentric revolution; conceptual dissatisfaction as driver of change                  |
| Tycho, Kepler, Galileo                | [[04-Tycho-Kepler-Galileo]]                   | Empirical challenge to metaphysics; abandoning perfect circles; mathematization of nature                                |
| Galilean Mechanics & Newton I         | [[05-Galilean-Mechanics-Newton-I]]            | Inertia; qualitative to mathematical explanation; law-governed nature                                                    |
| Newton II & Anomalies                 | [[06-Newton-II-and-Anomalies]]                | Universal gravitation; action at a distance; predictive power without full explanation; anomalies in successful theories |
| Pre-Darwin Natural History            | [[07-Pre-Darwin-Natural-History]]             | Cuvier's catastrophism; Lamarck's evolution; species fixity challenged                                                   |
| Darwin, Natural Selection & Evolution | [[08-Darwin-Natural-Selection-and-Evolution]] | Catastrophism vs uniformitarianism; Hutton, Lyell; Darwin's argument structure; philosophical significance               |
| Course Notes                          | [[Philosophy of Science Notes]]               | Consolidated notes across all lectures                                                                                   |

---

### 80261 Epistemology

| Name                                | Link                                                                                                    | Summary                                                                             |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Course Intro                        | [[00]]                                                                                                  | Course setup; readings list; intro to knowledge and justification                   |
| Overview                            | [[01 Overview]]                                                                                         | What epistemology is; justified true belief; philosophical vs scientific approaches |
| Empiricism                          | [[02 Empiricism]]                                                                                       | Knowledge from sensory experience; Hume; tabula rasa                                |
| Rationalism                         | [[03 Rationalism]]                                                                                      | Knowledge from reason; innate ideas; Descartes                                      |
| Locke                               | [[05 Locke]]                                                                                            | Locke's empiricist theory of knowledge; primary/secondary qualities                 |
| Locke vs. Leibniz                   | [[06 Locke vs. Leibniz]]                                                                                | Debate between empiricism and rationalism on the source of ideas                    |
| Hypotheses about Unobservables      | [[11 Hypotheses about Unobservables]]                                                                   | Scientific realism; how we reason about things we cannot directly observe           |
| Wesley Salmon on Scientific Realism | [[20 Wesley Salmon's Argument on Scientific Realism and Molecular Reality]]                             | Salmon's argument for molecular reality; inference to the best explanation          |
| Laws in Social Science              | [[21 Laws in Social Science - Dealing with Multiple Causation in Complex Phenomena]]                    | Causal complexity in social science; why universal laws are hard to establish       |
| Sandra D. Mitchell                  | [[22 Through the Fractured Looking Glass - Sandra D. Mitchell]]                                         | Integrative pluralism; science doesn't yield simple universal laws                  |
| Summary                             | [[Summary]]                                                                                             | Course summary of key epistemological positions and arguments                       |

---

### 80335 Social and Political Philosophy

| Name                                       | Link                                                                     | Summary                                                                                  |
| ------------------------------------------ | ------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------- |
| Mill — On Liberty                          | [[01 Mill - On Liberty]]                                                 | Harm principle; individual liberty; freedom of speech; tyranny of the majority           |
| Berlin — Two Concepts of Liberty           | [[02 Berlin - Two Concepts of Liberty]]                                  | Negative liberty (freedom from) vs positive liberty (freedom to)                         |
| Waldron — Homelessness                     | [[03 Waldron - Homelessness]]                                            | Property rights and homelessness; freedom and access to public space                     |
| Pettit — Republican Freedom                | [[04 Pettit - The Republican Ideal of Freedom]]                          | Republican freedom as non-domination; critique of negative liberty                       |
| Hesse — Black Fugitivity                   | [[05 Hesse - Western Hegemony, Black Fugitivity]]                        | Race and political freedom; Black fugitivity as resistance to Western hegemony            |
| Nozick — Anarchy, State & Utopia           | [[06 Nozick - Anarchy, State and Utopia]]                                | Libertarianism; minimal state; entitlement theory of justice                             |
| Cohen — Assessing Nozick                   | [[07 Cohen - Assessing Nozick's Libertarianism]]                         | Critique of Nozick; self-ownership; limits of libertarian theory                         |
| Rawls — Theory of Justice                  | [[08 Rawls - Theory of Justice]]                                         | Original position; veil of ignorance; difference principle; justice as fairness          |
| Harsanyi — Utilitarian Response to Rawls   | [[09 Harsanyi - A Utilitarian Response to Rawls]]                        | Utilitarian critique of Rawls; average vs maximin utility                                |
| Marxism                                    | [[10 Marxism]]                                                           | Class struggle; alienation; historical materialism; critique of capitalism                |
| Anderson — Against Luck Egalitarianism     | [[11 Anderson - Against Luck Egalitarianism]]                            | Critique of luck egalitarianism; democratic equality as an alternative                   |
| Lebron — Relational Egalitarianism & Race  | [[12 Lebron - Relational Egalitarianism & Race]]                         | Race and relational equality; structural racism and political philosophy                  |
| McKinnon — Epistemic Injustice             | [[13 McKinnon - Epistemic Injustice]]                                    | Testimonial and hermeneutical injustice; Miranda Fricker's framework                     |
| Young — Responsibility and Global Justice  | [[Young - Responsibility and Global Justice - A Social Connection Model]] | Social connection model; structural injustice; global responsibility                    |
| 当代西方政治哲学的八大流派                           | [[当代西方政治哲学的八大流派]]                                                     | Chinese-language overview of eight schools of contemporary Western political philosophy  |

---

### 85413 Perception

| Name                    | Link                        | Summary                                                                        |
| ----------------------- | --------------------------- | ------------------------------------------------------------------------------ |
| Introduction            | [[00 Introduction]]         | What perception is; physical input to neural signal; perception-action loop    |
| Introduction to Methods | [[01 Introduction to Methods]] | Psychophysics; signal detection theory; measuring perceptual thresholds     |
| Vision                  | [[02 Vision]]               | Visual system anatomy; light processing; object and depth perception           |
