Software Engineering: Principles, practices (technical and nontechnical) for confidently building high-quality software.

## Measurements and Metrics 

- Measurement: empirical, objective assignment of numbers, according to a rule derived from a model or theory, to attributes of objects or events with the intent of describing them

### Software Quality Metrics

- **Entities**: software product, modules, software development process, people
- **Software qualities**: Functionality (e.g., data integrity), Scalability, Security, Extensibility, Bugginess, Documentation, Performance, Installability, Availability, Consistency, Portability, Regulatory compliance
- **Process qualities**: Development efficiency, Meeting efficiency, Conformance to processes, Reliability of predictions, Fairness in decision making, Regulatory compliance, On-time release
- **People qualities**: 
	- Developers: Maintainability • Performance • Employee satisfaction and well-being • Communication and collaboration • Efficiency and flow • Satisfaction with engineering system • Regulatory compliance
	- Customers: Satisfaction • Ease of use • Feature usage • Regulatory compliance
- Non-trivial qualities: Software: Code elegance, Code maintainability; Process: Fairness in decision making; Team: Team collaboration, Creativity

### McNamara fallacy 

- Measure whatever can be easily measured. 
- Disregard that which cannot be measured easily. 
- Presume that which cannot be measured easily is not important. 
- Presume that which cannot be measured easily does not exist.

### Code Complexity

- **Lines of Code (LOC)**: A simple measure of how large a codebase is, but not always indicative of complexity or quality.
- **Halstead Volume**: Measures the size and complexity of a program based on the number of operators and operands.
- **Cyclomatic Complexity**: Measures the number of independent paths through a program and can indicate the number of test cases required for full coverage.
- **Object-Oriented Metrics**: Includes metrics like number of methods per class, depth of inheritance, and coupling between classes.
- **“Allowable mass” proxy**: More complex the software task -> consumes bigger part of the mass budget (in grams)
## How to use measurements and metrics? 

### Goal-Question-Metric (GQM) Framework

- **Goal**: Define what you want to achieve.
- **Questions**: Determine what you need to answer to know if your goal is met.
- **Metrics**: Identify what measurements are necessary to answer the questions.
- **Examples**:
    - **Goal**: Evaluate the effectiveness of a coding standard from team's perspective.
    - **Questions**: How comprehensible is the coding standard? What is its impact on team productivity?
    - **Metrics**: Number of revisions required to achieve compliance, team members' understanding, code size. 

#### Defining Goals
- **Purpose**: The reason or aim for defining the goal. It can be to improve, evaluate, or monitor something.
	- Example: Improve the reliability of the software.
- **Issue**: The specific aspect or challenge you're addressing. Common issues include reliability, usability, or effectiveness.
	- Example: Address the issue of poor usability in the current software design.
- **Object**: The target of the goal. This could be the final product, a specific component, a process, or an activity.
	- Example: Evaluate the usability of the user interface component.
- **Viewpoint**: The perspective from which the goal is assessed. It can be from the perspective of any stakeholder (e.g., user, developer, manager).
	- Example: Measure the usability from the perspective of end users.

### Measurement for Decision Making
- **Key Decisions Metrics Help With**:
	- **Fund project?**: Should we allocate resources and continue funding this project?
	- **More testing?**: Is the current testing sufficient, or do we need additional tests to ensure quality?
	- **Fast enough? Secure enough?**: Is the system performing efficiently and securely?
	- **Code quality sufficient?**: Is the codebase meeting the desired standards of quality?
	- **Which feature to focus on?**: Deciding which features require the most attention and improvement based on their importance.
	- **Developer bonus?**: Should performance metrics influence developer bonuses?
	- **Time and cost estimation? Predictions reliable?**: Are our time and cost estimations accurate, and are they backed by reliable data?

### Trend Analyses
- **Monitoring Test Result Trends**:![[Screen Shot 2024-10-06 at 21.00.37.png]]
	- The chart shows trends in test results over time, with the green areas representing passing tests and the red spikes indicating failures.
	- Regular trend analysis helps track improvements, regressions, or inconsistencies in the software over time.
	- **Purpose**: Trend analysis enables early detection of performance degradation or bugs and helps maintain a stable codebase.

### Benchmarking Against Standards
- **Monitor and Compare**: ![[Screen Shot 2024-10-06 at 21.01.19.png]]
	- **Monitor multiple projects or modules**: Keep track of different projects or code modules to establish typical values for metrics like test-to-code ratios.
	- **Report deviations**: Identify and report when a project deviates from established benchmarks or standards.

## Case study: Autonomous Vehicle Software 

By what metrics can we judge AV software (e.g., safety)?
1. Code Coverage
- **Definition**: Code coverage refers to the amount of code that gets executed during testing.
- **Types of Coverage**:
	- **Statement coverage**: Ensures individual lines of code are executed.
	- **Line coverage**: Measures whether specific lines of code are reached during testing.
	- **Branch coverage**: Ensures all possible branches in if-else conditions are covered.
- **Example**: 75% branch coverage means that 3 out of 4 possible outcomes in an if-else statement have been tested. 

2.  Model Accuracy
- **Definition**: This metric evaluates how accurately the machine learning models in AV software recognize objects, make decisions, or navigate environments.
- **Training**: The models are trained using labeled data, which includes sensor data (e.g., from cameras, radar) and corresponding ground truth.
- **Testing**: Accuracy is computed on a separate labeled test set.
- **Example**: If the model has 90% accuracy, it means that the object recognition is correct for 90% of the test inputs. 

3. Failure Rate
- **Definition**: Failure rate tracks the frequency of crashes or fatalities involving autonomous vehicles.
- **Measurement Units**: per 1,000 rides, per million miles, or per month.

4. Mileage
- **Definition**: Mileage is a measure of how many miles an autonomous vehicle has driven, which provides data on the software's real-world performance over time.
- **Importance**: The more miles an AV drives, the more data it accumulates to improve its algorithms, reduce failure rates, and increase reliability.
- **Example**: Waymo's vehicles have accumulated over 15 billion autonomously driven miles in simulation and over 20 million real-world miles on public roads. Such data is used to demonstrate the safety and reliability of AV software.

## Risks and challenges 

- **The streetlight effect**: A known observational bias. People tend to look for something only where it’s easiest to do so.
- **Making inferences**: Provide a theory (from domain knowledge, independent of data), Show correlation, Demonstrate ability to predict new cases (replicate/validate)
- **Spurious Correlations**: 
	- Confounding variables.
	- Berkson's paradox: when a dataset is biased or filtered in a certain way.
	- Survivorship bias: occurs when only the successes (or survivors) are considered in analysis. WWII plane example: focus on the areas without bullet holes.
- **Measurement Reliability**: The goal is to reduce uncertainty and increase consistency in measurements, which often requires multiple observations to account for variability.

## Metrics and incentives

- **Goodhart’s Law**: When a measure becomes a target, it can lose its effectiveness as a measure (e.g., focusing solely on lines of code might encourage bad coding practices).
- **Incentivizing Productivity**: Basing developer rewards on metrics (like number of bugs fixed or lines of code written) can lead to undesirable behaviors, such as writing unnecessary code or overlooking bugs.