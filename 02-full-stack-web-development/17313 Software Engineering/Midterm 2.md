

## 1) SE Ethics

- Human Flourishing: happiness and life satisfaction, mental and physical health, meaning and purpose, character and virtue, and close social relationships
    
- Algorithmic bias
    

Three questions to promote human flourishing

1. Does my software respect the humanity of the users?
    

- Humane design guide - Consider 6 human sensitivities: Emotional, Attention, Sense making, Decision making, Social Reasoning, and Group Dynamics 
    
- 1. In what ways does your product/feature currently engage Human Sensitivities? 
    
- 2. How might your product/feature support or elevate human sensitivities? 
    
- 3. Action Statement
    

2. Does my software amplify positive behavior, or negative behavior for users and society at large? 
    

- should have real humans to monitor and respond to your community.
    
- should have community policies about what is and isn’t acceptable behavior. 
    
- should have accountable identities. 
    
- should have the technology to easily identify and stop bad behaviors. 
    
- should make a budget that supports having a good community, or you should find another line of work
    

3. Will my software’s quality impact the humanity of others?
    

- Example: Malpractice vs negligence
    

## 2) ML in SE

ML Development: Observation, Hypothesis, Predict, Test, Reject or Refine Hypothesis ![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXfu_254NrPwSS_krAJ9xRSW_V7KR__J9pJ8MzTwkSecgZDT0owBR3MRJwaxD76Uobh_qQVyz27mnaXdrESCKqaSYHYLsD1gbrTxK71QvTs5hfBFfucMXzkdlW5XLxUg-r-gBAZiQQ?key=z7WFhs0byI7-wIAB8nf_gjfL)

Three Fundamental Differences between ML and SE

1. Data discovery and management:
    

- SE: Emphasis on designing algorithms and software logic; ML: More effort on discovering, transforming data
    

2. Customization and Reuse 
    

- SE: Can reuse modules; ML: Difficult to reuse same model as specific for certain tasks and datasets
    

3. No modular development of model itself
    

- SE: Can be monolithic or microservices; ML: ML models are usually monolithic
    

Feature Engineering: Identify parameters of interest that a model may learn on 

- Convert data into a useful form; Normalize data; Include context; Remove misleading things
    

  

Evaluation: Assess prediction accuracy on unseen data using metrics like false positives vs. false negatives for binary predictors (classification), error distance for numeric (regression), and top-K relevance for ranking.

### Mistakes & Mitigation

- Mistakes: System outage, Model outage, model not tested, deployment and updates not reliable, file corrupt, Model errors 
    
- Perform Hazard Analysis: What’s the worst thing that can happen? Backup strategy? Undoable? Nontechnical compensation?
    

Mitigating Mistakes

- Investigating in ML  e.g., more training data, better data, better features, better engineers  
    
- Less forceful experience 
    

- Instead of making the system automatically take actions, ask for user input (e.g., prompt the user for confirmation).
    
- Allow options to turn off certain features if they aren't working as expected.
    

- Adjust learning parameters e.g., more frequent updates, manual adjustments 
    
- Guardrails e.g., heuristics and constraints on outputs 
    
- Override errors e.g., hardcode specific results
    

### Quality attributes of ML models

- Interpretability (Explainability) 
    

- Model debugging 
    
- Auditing - fairness, safety, security 
    
- Trust 
    
- Actionable insights to improve outcomes - Helps extract useful knowledge from the model’s decisions to take better actions.
    
- Regulation
    

- Fairness 
    

- Group unaware: Ignore group data (one group could get excluded) 
    
- Group thresholds Different rules per group (rules differ by group) 
    
- Demographic parity: Same percentage in pool as outcomes (might result in random selection) 
    
- Equal opportunity: Equal chance out positive outcomes regardless of groups (focus on individual, rules differ per group) 
    
- Equal accuracy Equal chance of both outcomes per group (focus on group, rules differ per group)
    

- Inference latency 
    
- Inference throughput 
    
- Scalability
    

### Using LLMS 

Language Modeling: Measure probability of a sequence of words (Text sequence → Most likely next word)

#### Problems with LLMS

- Hallucinations: Factually Incorrect Output 
    
- High Latency: Output words generated one at a time, Larger models = slower 
    
- Output format: Hard to structure output (e.g. extracting date from text) 
    

- LLMs generate text in a natural language format, which can make it difficult to extract structured information like dates, numbers, or specific details.
    

#### Using LLMs for different daily tasks & Evaluation of Suitability

- Alternative Solutions: Ask if there's a more reliable or specialized tool than the LLM for your task.
    

- Example: For type-checking Java code, a Java compiler is a better choice because it's deterministic and built for the task.
    

- Error Probability: Estimate how often the LLM will give correct results for your problem. This may improve as LLMs get better but varies by task.
    

- Example: Grading mathematical proofs may have a higher error probability due to the complexity of logic.
    

- Risk Tolerance: Assess the consequences of mistakes made by the LLM.
    

- Example: Errors in answering emergency medical questions can be life-threatening, so tolerance for mistakes is low.
    

- Risk Mitigation Strategies: Identify ways to minimize errors or their impact.
    

- Example: For unit test generation, review and validate the LLM's output manually to catch mistakes before deployment.
    

#### Using LLMs as part of the codebase

→ Textual Comparison Tests to check for accuracy: 

- Syntactic Checks (correct structure and grammar); 
    
- Embeddings (Numeric representations of words that capture their meaning in context); 
    
- Cosine Similarity (Measures how similar two embeddings are based on the angle btw them)
    

#### Improving LLMs

- Prompt Engineering
    
- Chain of Thought Prompting
    
- Fine-Tuning
    

## 3) Dynamic Analysis / Advanced Testing

#### Correctness – Static Analysis and Testing 

#### Robustness – Fuzzing 

- What: Feed invalid, random, or unexpected inputs to a program to uncover vulnerabilities by observing crashes or abnormal behavior.
    
- Common Bugs:
    
- Causes: Invalid argument handling, type casting errors, untrusted code execution.
    
- Effects: Buffer overflows, memory leaks, division-by-zero, use-after-free, assertion failures.
    
- Impact: Affects security, reliability, performance, and correctness.
    

#### Performance – Profiling 

- Why? Identify performance bugs (e.g., slowdowns, degradation, cross-version/platform issues).
    
- Challenges: Define "fast enough," set thresholds, ensure reliable measurements. Bugs are hard to diagnose (e.g., system load, hardware, network, workflows) but impact user experience heavily.
    
- Profiling: Measures execution time and memory to find slow code (e.g., a "resize image" function taking 80% of time).
    
- Tracing: Tracks event sequences to debug crashes or unexpected behaviors (e.g., identifying the step causing failure).
    

#### Scalability – Stress testing 

- Why? To test the system's behavior beyond the limits of normal operation. Can apply at any level of system granularity. Often to test the error-handling capabilities of the application.
    
- How? throw large amounts of input / requests and see how the program behaves
    
- What it tests: Its breaking point. How well it recovers after failure.
    

#### Resilience – Soak testing 

- Why: Test system stability under extended, slightly above-normal load to detect issues like memory leaks or resource exhaustion. Useful for major releases or infrastructure changes, despite being time-consuming.
    
- How: Apply continuous load over time and observe performance.
    
- Tests: Long-term reliability and resilience to prolonged usage without memory leaks or performance degradation.
    
- Example: Simulating 500 users on an e-commerce site for 48 hours to find slowdowns or errors.
    

#### Reliability – Chaos Engineering 

- Why? To simulate a large-scale deployment and induce random failures in various components  – Test in Production with Chaos Engineering
    
- How:
    

1. Define baseline: Understand normal behavior (e.g., response time, uptime).
    
2. Induce failures: Turn off servers, break connections, or overload traffic.
    
3. Observe response: Check if the system recovers gracefully and quickly enough.
    
4. Fix weaknesses: Improve failover, load balancing, or other vulnerabilities.
    

- Benefits: Identify weak points before inevitable real failures; enhance system reliability.
    
- Examples:
    

- Google: Terminate networks or data centers to uncover hidden issues.
    
- Netflix: Use Chaos Monkey to randomly disrupt AWS instances or network links; monitor Stream Starts per Second (SPS) for availability.
    

#### Usability – A/B testing

- What: Controlled experiment with two variants (A = current system, B = new version).
    
- How: Randomly assign users to A or B and compare outcomes.
    
- Use: Common for testing web or GUI changes like ads or design layouts.
    

## 4) Feedback

- Feedback is composed of: 1. Appreciation; 2. Coaching (knowledge, skill);  3. Evaluation: where you stand, aligns expectations, and informs decision making
    
- Effective feedback: Goal-oriented; Actionable, Specific and Timely; Supportive, Truthful and Useful; Delivered privately in a neutral, non-judgemental tone
    
- Developmental Feedback
    

- Situation: Set the context. Help the person focus on what you are referring to.
    
- Behavior: Focus on the objective behavior to be repeated or changed.
    
- Impact: Share the direct impact of the behavior.
    
- Alternative: Share an alternative behavior to use next time.
    

## 5) Technical Debt 

- Internal Quality: code well structured; code understandable; codebase documented
    
- External Quality: software does not crash; software meet requirements; UI well designed
    

- Failure: Deviation of the component or system from its expected delivery, service or result. Manifested inability of a system to perform required function.
    
- Fault/defect: Flaw in component or system that can cause the component or system to fail to perform its required function. A defect, if encountered during execution, may cause a failure of the component or system.
    
- Error: A human action that produces an incorrect result. 
    
- What are bugs? Defects + Error
    

### Principles of Testing

1. Avoid the absence of defects fallacy: Testing reveals defects but cannot guarantee their absence or achieve 100% detection.
    
2. Exhaustive testing is impossible: cannot test all inputs. 
    
3. Start testing early: Begin testing early to guide design, get quick feedback, and catch bugs when they’re cheapest and least damaging to fix.
    
4. Defects are usually clustered: Focus testing on "hot" components with frequent changes, tricky logic, or high uncertainty, as defects tend to concentrate there.
    
5. The pesticide paradox: Repeating the same tests on evolving software misses subtler bugs, requiring varied testing methods for effectiveness.
    
6. Testing is context-dependent: Test goals and metrics depend on the specific requirements and acceptable risk levels of the project.
    
7. Verification is not validation: Verification checks if the software meets specifications, while validation ensures it meets the user's real needs.
    

### Test Design Techniques

- Exploratory Testing: Testing without scripts, using intuition to find bugs.
    

- Pro: Flexible, good for unexpected issues.
    

- Specification-Based ("Black Box") Testing: Tests based on specifications, ignoring code details.
    

- Pro: Avoids bias, robust to code changes, requires no code familiarity.
    

- Structural ("White Box") Testing: Tests designed with full code knowledge, focusing on structure.
    

- Pro: Ensures thorough code coverage.
    

- Exhaustive Testing:
    

- Issues: Need to be small enough to finish in a useful amount of time + Need to be large enough to provide a useful amount of validation
    
- Alternatives: Heuristics (Focus on the most likely or important scenarios)
    

- Equivalence Partitioning:
    

- What: Group inputs into equivalence classes with similar behavior and test one per class.
    
- Equivalence classes derived from specifications (e.g., cases, input ranges, error conditions, fault models) 
    
- Pro: Reduces test cases, requires domain knowledge.
    

- Boundary-value analysis
    

- Key Insight: Errors often occur at the boundaries of a variable value 
    
- For each variable, select: minimum, min+1, medium, max-1, maximum; possibly also invalid values min-1, max+1
    

- Pairwise testing (can find 50% - 90% defects)
    

- Key Insight: some problems only occur as the result of an interaction between parameters/components 
    
- E.g.: The bug occurs for senior citizens traveling on weekends (pairwise interaction) 
    

### Technical Debt

Organizations need to address the following challenges continuously:  1. Recognizing technical debt 2. Making technical debt visible 3. Deciding when and how to resolve debt 4. Living with technical debt

#### Types of Technical Debt:

1. Deliberate:
    

- Reckless: "We don’t have time for design."
    
- Prudent: "We must ship now and deal with consequences later."
    

3. Inadvertent:
    

- Reckless: "What’s layering?"
    
- Prudent: "Now we know how we should have done it."
    

## 6) Open Source Software

### Why Go Open Source (vs. Proprietary) ?

- Advantages 
    

- Transparency, gain user trust 
    
- Many eyes: crowd-source bug reports and fixes 
    
- Security: more likely for vulnerabilities to be quickly identified 
    
- Community and adoption: get others to contribute features, build stuff around you, or fork your project
    

- Disadvantages 
    

- Reveal implementation secrets 
    
- Many eyes: users can find faults more easily 
    
- Security: more likely for others to find vulnerabilities first 
    
- Control: You may not be able to influence the long-term direction of your platform
    

### License & Law

  

|   |   |   |
|---|---|---|
|License/Law|Key Purpose|Key Features|
|Copyright|Protects expressions of work|Automatic for books, music, code; exceptions for trivial ideas.|
|Intellectual Property (IP)|Protects ideas and inventions|Patents, machine designs, algorithms; licenses and expiry dates.|
|GNU General Public License (GPL)|Ensures software freedom|Four freedoms: use, change, share, and share modifications. Requires derivatives to use GPL.|
|Risks of GPL (Copyleft)|Enforces openness but can complicate usage|Derivatives must use the same license; companies may avoid GPL due to viral effects.|
|LGPL (Lesser GPL)|Allows use of libraries in proprietary code|Dynamic linking allowed; no derivative restrictions.|
|MIT License|Simple, commercial-friendly open-source|Must credit author; no liability; no restrictions on usage.|
|Apache License|Industry-friendly open-source|Allows use without source code sharing; no trademark permissions.|
|BSD License|Minimal restrictions|Requires copyright notice; no liability; allows modifications freely.|
|Creative Commons (CC)|Licensing for non-code content|Used for datasets, images, videos, documentation, etc.|

  

## 7) Dependency Management

What is a Dependency?

- Core of what most build systems do. 
    

- Example: Foo->Bar: To build Foo, you need to built Bar. "Bar" is a dependency of "Foo."
    
- Scopes:
    

- Compile: Use Bar’s classes/functions during compilation.
    
- Runtime: Use abstract APIs provided by Bar during execution (e.g., logging, database).
    
- Test: Use Bar for testing only (e.g., JUnit, mocks).
    

- Internal: Built/maintained by your organization.
    
- External: Downloaded via package managers
    
- Dependencies are typically hosted on servers and downloaded using package managers, requiring unique identifiers for each package. 
    
- Most package managers support custom repositories, which require proper management.
    

### Dependency Pinning vs. Floating

- Pinning Dependencies (e.g. 1.5.3) - Specific version of the dependency. Frozen in time. 
    

- Pro: Reproducible builds; Stable network effects
    
- Con: Can become vulnerable due to dependency bugs; Have to keep updating dependents as dependencies evolve
    

- Floating Dependencies (e.g. 1.x) - Each build will pull the latest available libFoo version 
    

- Pro: Latest security patches & bug fixes; Less manual maintenance 
    
- Con: Flaky builds (breaking changes) ; Floats leak transitively (A pin to B floating C; then A still sees changing version of C) 
    

### Types of Dependencies

1. Transitive Dependencies: a dependency that your software indirectly relies on (a dependency of your dependency)
    
2. Diamond Dependencies: multiple intermediate dependencies have the same transitive dependency.
    

- Problem: Different intermediate dependencies may require different versions of the same transitive dependency.
    

3. Cyclic Dependencies: Avoid at all costs, but sometimes unavoidable or intentional (e.g. GCC is written in C - needs a C compiler; Apache Maven uses the Maven build system)
    

**