- You will never understand the entire system!

How do I tackle this codebase? 
- Leverage your previous experiences (languages, technologies, patterns) 
- Consult documentation, whitepapers 
- Talk to experts, code owners 
- Follow best practices to build a working model of the system

How to tackle codebases 
- Goal: develop and test a working model or set of working hypotheses about how (some part of) a system works 
- Working model: an understanding of the pieces of the system (components), and the way they interact (connections) 
- Observation, probes, and hypothesis testing

1. Look at `README.md `
2. Clone the repo. 
3. Build the codebase. 
4. Figure out how to make it run. 
5. What do you want to mess with? • Clone and own 
6. Traceability - Attach a debugger 
	1. View Source 
	2. Find the logs. 
	3. Search for constants (strings, colors, weird integers (`#DEADBEEF`))

## Observations

- Software is full of patterns (File structure • System architecture • Code structure • Names)
- Software is massively redundant (Theres always something to copy/use as a starting point)
- Code must run to do stuff!
- If code runs, it must have a beginning…
- If code runs, it must exist…

### The Beginning: Entry Points 

- Locally installed programs: run cmd, OS launch, I/O events, etc. 
- Local applications in dev: build + run, test, deploy (e.g., docker) 
- Web apps server-side: Browser sends HTTP request (GET/POST) 
- Web apps client-side: Browser runs JavaScript, event handlers

### Code must exist. But where? 

- Locally installed programs: run cmd, OS launch, I/O events, etc. 
	- Binaries (machine code) on your computer
- Local applications in dev: build + run, test, deploy (e.g., docker) 
	- Source code in repository (+ dependencies) 
- Web apps server-side: Browser sends HTTP request (e.g., GET, POST) 
	- Code runs remotely (you can only observe outputs) 
- Web apps client-side: Browser runs JavaScript, event handlers 
	- Source code is downloaded and run locally (see: browser dev tools!)

### Information Gathering 

- Basic needs: 
	- Code/file search and navigation 
	- Code editing (probes) 
	- Execution of code, tests 
	- Observation of output (observation)

### Probes: Observe, control or “lightly” manipulate execution 

- print(“this code is running!”) 
- Structured logging 
- Debuggers 
	- Breakpoint, eval, step through / step over 
	- (Some tools even support remote debugging) 
- Delete debugging 
- Chrome Developer Tool