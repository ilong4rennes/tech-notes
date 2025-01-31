- **Static and Dynamic Analysis**:
	- **Static analysis**: Examines source code without executing it.
		- Conservatively reasons about all possible inputs and program paths; Reported warnings may contain false positives
	- **Dynamic analysis**: Examines code during execution, requires successful build + test inputs, useful for profiling, coverage analysis, and testing. 
		- Observes individual executions; Reported problems are real, as observed by a witness input
		- Cannot find problems in unexplored parts of code, so it’s dependent on test cases. Subject to false negatives

- **What Static Analysis Can and Cannot Do**:
	- **What it can do**:
		- Prevent type errors.
		- Detection of problematic syntax patterns (e.g., using `==` for strings, array index out of bounds).
		- Detect certain defects: 
			- Security: Buffer overruns, improperly validated input… 
			- Memory safety: Null dereference, uninitialized data… 
			- Resource leaks: Memory, OS resources…
	- **What it cannot do**:
		- Cannot reason about termination (halting problem is undecidable).
		- Cannot provide definite results for complex or exact value reasoning.
		- Cannot advanced properties.

- **Static Analysis Limitations**
	- **Rice's Theorem**: Some properties (like termination) are undecidable. Static analysis has inherent limitations and will over-approximate issues.

- **What makes a good static analysis tool?** 
	- fast, report few false positives, be continuous (Should be part of your continuous integration pipeline, Diff-based analysis is even better -- don’t analyze the entire codebase; just the changes); informative

- **Tools for Static Analysis**
	- **Linters**: Focus on code style and shallow bug detection.
		- Examples: Enforcing proper indentation, naming conventions, documentation of functions, etc.
		- Cheap, fast, enforce style and improve readability.
	- **Pattern-based Bug Detectors**: Identify common errors based on language patterns or API usage.
		- Examples: Comparing strings with `==` (wrong practice in Java), improper handling of exceptions, inefficient string concatenation.
		- Detect deeper issues but require tuning to avoid false positives.
	- **Type-annotation Validators**: Use type annotations like `@NonNull` and `@Nullable` to prevent null pointer exceptions and other errors.
		- Useful in languages that support or require strong typing.

- **Integrating Static Analysis in CI/CD**
	- Static analysis should be fast, have low false positives, and help developers pinpoint issues.
	- Integrated tools should suggest or automatically apply fixes.

- **Challenges of Static Analysis**
	- **False Positives**: Too many false positives will cause developers to ignore warnings.
	- **Performance**: If the analysis is too slow, it hinders development.
	- **Complexity**: Deep analysis requires a balance between correctness and speed.

- **Best Practices**
	- Use a combination of static and dynamic tools to achieve high code quality.
	- Maintain focus on fast and efficient tools to integrate into CI systems.