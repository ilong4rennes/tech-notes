### 1. Risk Management

- **Definition of Risk:** a measure of the potential inability to achieve overall program objectives within defined cost, schedule, and technical constraints
- **Components of Risk:**
	- Likelihood of failure to achieve a particular outcome.
	- Consequences of failing to meet objectives.
- **Internal vs. External Risks:** Internal risks are controllable, while external risks are uncontrollable.

- **Risk Management Levels:**
	- 1. Crisis management: Fire fighting; address risks only after they have become problems. 
	- 2. Fix on failure: Detect and react to risks quickly, but only after they have occurred. 
	- 3. Risk mitigation: Plan ahead of time to provide resources to cover risks if they occur, but do nothing to eliminate them in the first place. 
	- 4. Prevention: Implement and execute a plan as part of the software project to identify risks and prevent them from becoming problems. 
	- 5. Elimination of root causes: Identify and eliminate factors that make it possible for risks to exist at all.

- **Risk Assessment Matrix:** ![[Screen Shot 2024-10-07 at 09.36.00.png]]
- **Risk Prioritization:**
  - Prioritize risks based on exposure and impact on the project.

### 2. Risk Mitigation Techniques

- **DECIDE Model:**
	- Detect that the action necessary 
	- Estimate the significance of the action 
	- Choose a desirable outcome 
	- Identify actions needed in order to achieve the chosen option 
	- Do the necessary action to achieve change 
	- Evaluate the effects of the action

- **Swiss Cheese Model:** 
	- Describes how different layers of failures can align to cause major issues.
- **Pre-mortems:**
	- Assume failure has already happened and work backward to identify possible causes.
- **OODA Loop:**
	- Decision-making framework: Observe, Orient, Decide, Act.
- **Why do we make mistakes?**
	- Generalization: Our brains generalize simple, component parts to focus on complex tasks
	- Cognitive Load

### 3. Automation and CI/CD Pipelines
- **Can we catch human error?:**
	- Automate what we can - Use CI to catch your mistakes, make you look better, and mitigate your risks
	- Review what we cannot - Use Code review to teach and learn
- **CI/CD Pipeline Overview:**
	- Code Edit -> Tests run -> Code merged -> Code deployed
- **Example CI/CD Pipeline**: ![[Screen Shot 2024-10-07 at 10.32.15.png]]
- **Benefits of CI:** Catch bugs early, improve productivity, ensure high-quality code.
- **Static Validation:** Tools like static analysis and style guides catch errors during development.
- **Checklist for Process Management**: vital for managing complex tasks.
	- Start with problems we have seen before 
	- Justify why this is not automatable 
	- Not all checklist items need to be very specific 
		- An item could be “does this team know we are proposing this change”
  
### 4. Code Review and Process Management

- **Code Review Overview:**
	- Code reviews help identify issues automation cannot, improving code quality.
- **Mismatch of Expectations and Outcomes** 
	- Low quality of code reviews: Reviewers look for easy errors, miss serious errors
	- Understanding (reason, code, context) is the main challenge • 
	- No quality assurance on the outcome
- **Style Guides:** Important for maintaining code consistency across a team or project.
- **Expectations for Code Reviews:**
	- Start with the “big ideas”
	- Automate the little things 
	- Focus on understanding  
	- Remember a person wrote the code 
	- Don’t overwhelm the person with feedback
- **Code Review Practices at Google:** Force developers to write code that other developers could understand; Ensures code consistency, thorough tests, and avoidance of arbitrary changes.
- **Reviewing Relationships**: ![[Screen Shot 2024-10-08 at 01.37.09.png]]
- **Don’t forget that coders are people with feelings**