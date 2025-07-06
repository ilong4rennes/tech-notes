# Application Development Fundamentals 

- Software Development 
	- ![[Screen Shot 2024-02-26 at 04.29.49.png]]
- Top five project success factors
	- ![[Screen Shot 2024-02-26 at 04.34.47.png]]
	- 1. User involvement
	- 2. Executive Support
	- 3. Clear Business Objectives
	- 4. Emotional Maturity
- Capability Maturity Model Integration (CMMi)
	- a framework of best practices aimed at improving the performance, efficiency, and quality of an organization's processes
	1. Establish Quantitative Objectives. 
	2. Measure Process Performance. 
	3. Use Statistical Process Control (SPC). 
	4. Conduct Root Cause Analysis. 
	5. Optimize Processes. 
	6. Encourage Innovation. 
	7. Monitor and Repeat.
- Project resources
	- ![[Screen Shot 2024-02-26 at 04.54.23.png]]
	- TCO: full cost of owning a product
	- Many important future costs are ignored: Continued training • System updates and improvement • Auditing systems to ensure compliance • Security • Backup of critical data • Disaster Recovery
- Team Dynamics
	- ![[Screen Shot 2024-02-26 at 04.56.07.png]]
- Risk 
	- concerns the deviation of one or more results of one or more future events from their expected value![[Screen Shot 2024-02-26 at 04.59.21.png]]

# SDLC Methodologies
## 1. The Software Wars: Why You Can’t Understand your Computer
1. We can’t understand our computer 
- Rapid changes in technology (Moore’s Law) 
- Software becomes obsolete as soon as we master it 
- Developer and business expert communication (“interdependent parasitism”) is complex 
- Too ambitious of a timeline that is rarely achieved in practice and keeps extending 
	- Lack of visibility (code walk through, timeline) 
	- 90% Syndrome describes a project that reaches about 90% completion on schedule, then stalls, finally finishing after about twice the originally projected duration.

2. The Mythical Man-Month 
- Any increased productivity achieved by hiring more people gets nibbled at by the increased complexity of communication among them. 
- A system that one person can develop in thirty days cannot be developed in a single day by thirty people.

3. Why we will never have reliability? 
- Programmers' Affinity for Complexity
- Difficulty in Oversight
- Evolving System Specifications

best practices suggested by DePalma 
1. Keep it simple. 
2. Avoid exotic and new programming techniques. 
3. Know that an army of workers is no substitute for clear design and ample time. 
4. Hire honest people 
5. Make only modest promise

## 2. Systems Development Life Cycle (SDLC)
- Where Do IS Projects Come From? Fulfill a business need
- Common Stages in Systems Development
	- ![[Screen Shot 2024-02-26 at 05.24.44.png]]

### Build / Buy (Control / Cost Tradeoff)
- Buy 
	- Ready much sooner 
	- Less control - May not exactly be what you want 
	- Tend to be expensive if customization is needed 
	- Makes you dependent upon the vendor. 
	- Options for Buying: 
		1. Purchase commercial systems 
		2. Contract with third party, i.e, outsource 
		3. Strategic alliances
- Build it yourself 
	- More control – customizable (often scalable) 
	- May take longer and may be more expensive 
	- Depends on in-house expertise and resources 
	- May also provide competitive advantage (Think AWS)
	- How to Build? 
		1. Structured Approaches
		2. Rapid Application Methods 
		3. Agile Methods

### System Development Methodologies
- Phases in SDLC
	- ![[Screen Shot 2024-02-26 at 05.01.10.png]]
- SDLC Methodologies
	1. Structured Systems Development (e.g., Waterfall - Sequential) 
		- Separate and distinct phases of specification and development. 
		- doing each phase thoroughly (and welldocumented) before moving forward ensures correct and high-quality outcomes
		- Strength: Frozen requirements / reliable
		- Weakness: Long time / Less user involvement in early stages
	2. Rapid Application Development (e.g., Iterative) 
		- Develop multiple versions of the system and evaluate with user until a final working system is arrived at 
		- A series of alternate versions developed sequentially
		- Specification, development and validation are interleaved. 
		- Strength: Quick user feedback
		- Weakness: Users face incomplete system / must be patient
	3. Agile Development Methodology (e.g., Scrum - Incremental) 
		- The system is assembled incrementally with emphasis on accommodating change (iterative & incremental)
		- agility: the ability to both create and respond to change in order to profit in a turbulent business environment
		- Minimize risk -> short iterations 
		- Agile Methodologies
			- Scrum
			- Kanban
			- Xtreme Programming
		- Strength:
			- Fast delivery of results (short time schedule)
			- Works well in projects with undefined or changing requirements
		- Weakness:
			- Requires discipline 
			- Significant user involvement is essential 
			- Initial high learning curve 
			- Works best in smaller projects
		- ![[Screen Shot 2024-02-26 at 07.49.00.png]]

# SDLC Modeling - Structured Approaches

Phase 1: Planning
- Define the system - build / buy
- Define the scope - prevent scope creep (occurs when the scope of the project increases (e.g., CRM became a full-fledged Sales and Advertising system))
- Define the project plan - tasks / time / resources
- PM tools: Basecamp, Kanboard, Trello

Phase 2: Analysis
- Document system requirements (e.g., Use Cases)
- Understand the business processes
- Prioritize the requirements
	- Prevent Feature creep - occurs when developers add extra features that were not part of the initial requirements (e.g., Add customized reports in addition to standardized reports
- UML Use cases
	- ![[Screen Shot 2024-02-26 at 08.02.44.png]]
	- ![[Screen Shot 2024-02-26 at 08.02.55.png]]
	- ![[Screen Shot 2024-02-26 at 08.03.06.png]]
	- ![[Screen Shot 2024-02-26 at 08.03.15.png]]
	- ![[Screen Shot 2024-02-26 at 08.05.25.png]]

Phase 3: Design
- Technical architecture (hardware, software, telecommunication)
- UI
- Software architecture (classes & interrelationships, db design)
	- ER diagrams![[Screen Shot 2024-02-26 at 08.12.48.png]]
	- Wireframes (LoFi, HiFi)

Phase 4: Development
- Build system
- Build software (db, programs, UI)

Phase 5: Testing
- Healthcare.gov case
	- What problems did the Healthcare.gov site face on the first day of launch? 
		- Although only about 50000 to 60000 potential visitors were expected on the launch day, over 250,000 users were said to have visited the Healthcare.gov site on launch day 
		- The system was filled with flaws and bugs that only eight people were enrolled on the first day 
		- Users encountered significant delays in accessing information on the site 
		- Many registrations failed and users had to enter information all over again.
	- What factors contributed to the problems faced by the Healthcare.gov site on the day of its launch? 
		- Poor planning (not involving technology folks) and lack of ownership (responsibilities assigned) 
		- Starting design and implementation phase before requirements were complete 
		- Major design flaws leading to usability issues 
		- Lack of capacity planning 
		- Absence of thorough testing before deployment
- Testing Methods
	- ![[Screen Shot 2024-02-26 at 08.18.57.png]]

Phase 6: Implementation
- distribute the system to the targeted users and they begin using the system to perform their everyday jobs 
- Change management is an important aspect of this phase - This is where DevOps becomes relevant
- Write detailed user documentation 
	- User documentation - highlights how to use the system 
- Provide training for the system users 
	- Online / Workshop training 
- Choose the right implementation method 
	- Parallel implementation – use both the old and new system simultaneously 
	- Plunge implementation – discard the old system completely and use the new 
	- Pilot implementation – start with small groups of people on the new system and gradually add more users 
	- Phased implementation – implement the new system in phases

Phase 7: Maintenance & Sustainability 
- Maintenance phase - monitor and support the new system to ensure it continues to meet the business goals 
	- Build a help desk to support the system users
		- Help desk - a group of people who responds to knowledge workers’ questions 
	- Provide an environment to support system changes 
		- Resolve bugs and errors • Enhancements • Upgrades

- Examples of Tools to Support SDLC
	- ![[Screen Shot 2024-02-26 at 08.25.25.png]]
# SDLC Modeling - Agile Approaches

Agile Manifesto - Four Principles 
1. Responding to change – over following a plan 
2. Individuals and interactions – over processes and tools 
3. Working software – over comprehensive documentation 
4. Customer collaboration (or work with users) – over contract negotiation

## Scrum – Agile Software Management
- Product Backlog & Owner
	- Product Owner 
		- – Sole owner of the product backlog 
		- – Changes to the product backlog have to be approved by the product owner 
		- – Technical lead or Project Manager 
	- Product Backlog 
		- – List of features under consideration 
		- – Business features and technology features 
		- – Sorted by priority
- Scrum Master & Team
	- Scrum Master 
		- – Interface between the management and the scrum team 
		- – Typically, an experienced engineer 
		- – Responsible for removing blocks that stall the progress of Scrum Team Members 
		- – Should be able to make quick decisions based on incomplete data 
	- Scrum Team 
		- Cross Functional 
		- Subject experts, Designers, Testers, Technical Writers? 
		- Recommended Team Size 5 - 10
- Sprint
	- Lasts for about 30 days 
	- Implement the top priorities in the Project Backlog called the Sprint Backlog 
	- Sprint estimates updated as tasks are completed or new tasks crop up 
	- Potentially shippable product increment
- Standup Meeting
- Sprint Review

## Personas & User Stories in User Centered Design
- As a user role, I want/need/can some goal so that reason/benefit
- Do not capture design details or features of application

# Building Web Applications

![[Screen Shot 2024-05-05 at 20.18.51.png]]
3-tier:
![[Screen Shot 2024-05-05 at 20.19.00.png]]
## Web Application Programming
- Front-end (browser side) – JavaScript, Java etc. 
- Back-end (Server side) – Java, JavaScript, ASP.NET, C#, PHP, Ruby, Python etc. 
- Back-end (Database - Oracle, MySQL, SQL Server, NoSQL ) – SQL

**Separation of content, layout and behavior**
![[Screen Shot 2024-05-06 at 04.41.50.png]]
## HTML

### HTML Tags
![[Screen Shot 2024-05-05 at 20.26.06.png]]

### HTML Structure
```
<!DOCTYPE html>
 <html lang="en">
 <head> --metadata
	 <meta charset="UTF-8">
	 <meta name="viewport" content="width=device-width, initial-scale=1.0">
	 <title>Document</title> --important for SEO
 </head>
 <body> --displayed to screen
	 <!-- Content goes here -->
 </body>
</html>

```

### HTML Elements
![[Screen Shot 2024-05-05 at 20.28.51.png]]
- `<a href="url" target="_blank">link text</a>`
	- target attribute: specifies where to open the linked document
	- A. `_self` - Default. Opens the link in the same window/tab as it was clicked 
	- B. `_blank` - Opens the link in a new window or tab 
	- C. `_parent` - Opens the link in the parent frame 
	- D. `_top` - Opens the link in the full body of the window
- `<img src=“path“ alt=“text” >`

### Attributes
- **Use `class` when**:
    - You want to apply the same style to multiple elements.
    - You want to apply multiple styles to the same element.
    - You are using CSS frameworks that depend on class selectors (like Bootstrap). `.highlight {}`
- **Use `id` when**:
    - You need to uniquely identify an element for JavaScript interactions (like getting or setting the element’s content).
    - You have a specific style that should only apply to one element.
    - You are creating page anchors that you can link to (e.g., `#section-name` in a URL).

### HTML Table
```
<table border="1">
	<tr>
		<th>Header 1</td>
		<th>Header 2</td> --text in a element is bold and centered by default
	</tr>
	<tr>
		<td>row 1, cell 1</td>
		<td>row 1, cell 2</td>
	</tr>
	<tr>
		<td>row 2, cell 1</td>
		<td>row 2, cell 2</td>
	</tr>
</table>
```

### HTML Form

A form can contain input elements like: 
- `<input type="__">` 
	- text (text field with default width 20 characters)
	- checkbox (select an option)
	- password (passwords are masked)
	- radio = allows users to select one of a set of options
	- submit
- `<select>` select lists and `<option> <optgroup> `
- `<button>` submit buttons

```
<form action="">
 <label for="name"> Your Name (required):</label> 
 <input type="text" id="name" required/>
 <label for="email"> Your Email (required):</label>
 <input type="email" id="email" required>
 <label for="comment"> Your Comment or Question:</label>
 <textarea id="comment" cols="30" rows="10" required></textarea>
 <input type="submit" value="SEND"/>
</form>
```
- Input fields should be associated with labels or instructions
![[Screen Shot 2024-05-06 at 03.42.27.png]]

### List

Unordered:
```
<ul>
	<li>Coffee</li>
	<li>Tea</li>
	<li>Milk</li>
</ul>
```

Ordered:
```
<ol>
	<li>Coffee</li>
	<li>Tea</li>
	<li>Milk</li>
</ol>
```

## CSS
- defines how to render HTML elements
- `<link rel="stylesheet" href="mystyle.css">`
- style rules: ![[Screen Shot 2024-05-06 at 03.47.36.png]]
- Order in which stylesheets are applied: 1. Browser default 2. External style sheet 3. Internal style sheet (in the head section) 4. Inline style (inside an HTML element)

Types of Selectors ![[Screen Shot 2024-05-06 at 05.00.05.png]]
- id Selector (Select a single unique element), defined with a "#"
- Class Selector (Select a group of elements), defined with a "."

Box Model ![[Screen Shot 2024-05-06 at 05.01.10.png]]
![[Pasted image 20240506051515.png]]
- `style="margin: 50px;”` margin-top margin-right margin-bottom margin-left
- `Style=“padding: 50px;”` padding-top padding-right padding-bottom padding-left
- `Style=“border: 5px solid red”` Width Style Color

**Centering Elements**
- To horizontally center a block element (like `<div>`), use `margin: auto;`
- To just center the text inside an element, use `text-align: center;`

**Combinators Selectors** 
I. descendant selector (space) 
II. child selector (>) 
III. adjacent sibling selector (+) 
IV. general sibling selector (~)
V. Descendant Selector (`*`)

- em is always relative to font size. 
- % is relative to the containing block
## JavaScript
- `<script src= "../static/script.js" type="text/javascript"> </script>`
- `<script defer src= "../static/script.js"> </script>`
	- If defer is set the script is executed after the page has finished parsing
- events: `<element event='some JavaScript code or function'>`
	- `<button onclick='alert("Caution-it is full moon")'> Alert </button>`
- document object access
	- `<button onclick='alert(document.getElementById("hello").innerText)';>Alert </button>`
	- `<button onclick='alert(document.getElementsByTagName("p")[1].innerText)';> Read Paras </button>`

## User Testing
Usability Based on Five Dimensions (5E)
- effective
- efficient
- engaging
- error-tolerant
- easy-to-learn

Think-Aloud Protocol: 
- No leading questions. Let the user lead the way.
- Reassure the user that you are not testing them, you are testing the software 
- Having them talk out loud is non-intuitive! Ask them to keep speaking to understand what they’re thinking 
- Don’t start defending or explaining the software, be hands off, have them explain what they’re doing

## Web Accessibility
- Web Content Accessibility Guidelines (WCAG)
- POUR Principles: perceivable, operable, understandable, robust
- The alt must be used at all times
- fixed pixel size is not a good choice for responsive design