- **How to develop software?**
	- Discuss the software that needs to be written 
	- Write some code 
	- Test the code to identify the defects 
	- Debug to find causes of defects 
	- Fix the defects

- **Improving Process Reliability**:
	- Write down all requirements and require approval for all changes to requirements.
	- Use version control and code reviews.
	- Track all work items 
		- Break down development into smaller tasks 
		- Write down and monitor all reported bugs 
		- Hold regular, frequent status meetings
	- Plan and conduct quality assurance
	- Employ a DevOps framework to push code between developers and operations
	- ![[Screen Shot 2024-10-07 at 05.30.13.png]]
	- Hypothesis: Process increases flexibility and efficiency 
	- Ideal Curve: Upfront investment for later greater returns

- **Cost of Defects Based on Creation and Correction Phases**:
	- Defects created earlier in the project (during requirements or design phases) are much cheaper to fix than those created later (during construction or maintenance).
	- The cost to correct defects grows exponentially the later they are discovered and addressed.

-  **Waterfall Model**: This slide introduces the Waterfall model, which was the original software process. The steps include:
	- Requirements
	- Design
	- Implementation
	- Verification
	- Maintenance
	- Each phase flows into the next, similar to a waterfall, where each stage must be completed before the next begins.

- **Lean Production and Toyota Production System (TPS)**:
	- Toyota Production System (1940s) introduced lean production concepts.
	- The focus is on building only what is needed, using a "pull" system (Kanban) to avoid overproduction.
	- The system emphasizes fixing problems early, empowering workers to take ownership, and continuous improvement (kaizen).

- **From TPS to Agile & Scrum**: Modern processes focus on flexibility and iterative development, breaking down tasks into sprints with frequent feedback loops.

- **Elements of Scrum**: ![[Screen Shot 2024-10-07 at 06.06.53.png]]

	- **Backlogs**
		- Product Backlog: List of all features needed for the product.
		- Sprint Backlog: Features/tasks to be completed in the current sprint, broken down into smaller, manageable tasks (fine-grained, estimated, assigned to members).
		- User stories are commonly used to describe tasks.
	- **Kanban Boards** (Project Board in GitHub)
		- Visual representation of the workflow, often divided into columns like "Backlog," "In Progress," and "Done."
		- Helps teams track progress and identify bottlenecks in the process.
	- **Scrum Meetings**
		- **Sprint Planning**: The team decides what to tackle in the upcoming sprint.
		- **Daily Scrum**: A quick meeting to address what was done, what’s next, and any obstacles.
		- **Sprint Retrospective**: Reviews the process and suggests improvements.
		- **Sprint Review**: Focuses on reviewing the product. 
  
- **User Stories**:
	- Describes features from a user’s perspective: "`As a [role], I want [function], so that [value].`"
	- User stories - INVEST framework
		- Independent 
			- Schedule in any order. 
			- Not always possible
		- Negotiable 
			- Details to be negotiated during development 
			- Good Story captures the essence, not the details
		- Valuable 
			- This story needs to have value to someone (hopefully the customer)
		- Estimable 
			- Helps keep the size small 
			- It should provide enough details to estimate the amount of effort needed
		- Small 
			- Fit on 3x5 card 
			- At most two person-weeks of work (one sprint) 
			- Too big == unable to estimate
		- Testable 
			- Ensures understanding of task 
			- We know when we can mark task “Done” 
			- Unable to test == do not understand

| Principle  | Violating Story | Why it Violates | Fixed Version |
|------------|-----------------|-----------------|---------------|
| Independent | As a user, I want to receive notifications for events after the event calendar is set up, so I don’t miss any events. | This story depends on the development of the event calendar, making it not independent. It cannot be worked on until the calendar feature is completed. | As a user, I want to receive notifications for events I’ve shown interest in, so I don’t miss them. |
| Negotiable | As a user, I want a button that sends email notifications for every event 30 minutes before the start time, and it should include the event name, location, and duration. | The story is too specific and doesn’t allow room for negotiation. The development team has no flexibility to discuss other options. | As a user, I want to receive a reminder about upcoming events, so I can be notified before they begin. |
| Valuable | As an admin, I want to export user data to a CSV file. | This story lacks clarity about why the feature is valuable to the user. It focuses on a technical feature without explaining its purpose. | As an admin, I want to export user data to a CSV file, so I can analyze user activity trends and make data-driven decisions. |
| Estimable | As a user, I want the app to be fast and reliable at all times, so I can have a smooth experience. | The story is too vague and broad. It’s impossible to estimate the effort required without more specific criteria or measurable goals. | As a user, I want the app to load the main dashboard in under 2 seconds, so I can access the information quickly. |
| Small | As a user, I want to manage my profile, upload a profile picture, and update my privacy settings from the settings page. | The story is too large and covers multiple features. This would be difficult to complete in one sprint and needs to be broken down. | As a user, I want to update my privacy settings, so I can control who sees my information. |
| Testable | As a user, I want the app to work well on all devices. | The story is too vague and doesn't have clear acceptance criteria, making it difficult to test. | As a user, I want the app’s homepage to display correctly on mobile devices, so I can easily navigate it on my phone. |

- **Improving Time Estimates**
	- Prevent conformity bias.
	- Consider past comparable experiences to base an estimate on.
	- Determine how much design is needed for each task.
	- Break tasks down into smaller tasks and estimate those.

- Milestones and deliverables make progress observable 
	- Milestone: clear end point of a (sub)tasks 
		- For project manager 
		- Reports, prototypes, completed subprojects
		- "80% done" is not a suitable milestone 
	- Deliverable: Result for customer 
		- Similar to milestones, but for customers 
		- Reports, prototypes, completed subsystems

- **Time Estimation and Milestones**:
	- Time estimation is difficult but necessary for planning.
	- Break tasks into smaller parts to improve estimation accuracy.
	- Milestones mark clear endpoints in a project and make progress measurable.