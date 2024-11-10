## Step 1: Repository Setup

### 1.1 Navigate to the Repository 
- [https://github.com/CMU-313/NodeBB](https://github.com/CMU-313/NodeBB).
### 1.2 Fork the Repository
- Click the Fork button
- GitHub will create a copy of the repository under your GitHub account. 
### 1.3 Clone the Fork
- Clone your fork to your local machine to make changes.
- `git clone https://github.com/your-username/NodeBB.git`
### 1.4 Push Changes and Create Pull Requests (Optional, see below)
- Push: `git push origin main`
- If you need to contribute back to the original repository, you can create a pull request from your fork on GitHub.

## Step 2: Installing NodeBB on Mac

### 2.0 Required software: node.js, brew, redis
- start the redis server: `redis-server`
### 2.1 Installing NodeBB
- Enter the directory where you have cloned the repository: `% cd NodeBB`
- Run the interactive installation command: `% ./nodebb setup`
- `Which database to use (mongo) redis`
### 2.2 Start the NodeBB server
- `./nodebb start`
- You can now visit your forum at [`http://localhost:4567/`](http://localhost:4567/).

## Step 3: Lint and Test

(Purpose: prevent introducing unexpected bugs for other developers when working on an existing codebase in a collaborative setting)

### 3.0 Install `npm`
- `npm install`
### 3.1 Run the linter
- `npm run lint`
### 3.2 Add a test database in `config.json`

Original: 
```json
{ 
	"url": "http://localhost:4567", 
	"secret": "qq", 
	"database": "redis", 
	"redis": { 
		"host": "127.0.0.1", 
		"port": "6379", 
		"password": "", 
		"database": "0" 
	}, 
	"port": "4567" 
}
```

After change:
```json
{
    "url": "http://localhost:4567",
    "secret": "qq",
    "database": "redis",
    "redis": {
        "host": "127.0.0.1",
        "port": "6379",
        "password": "",
        "database": "0"
    },
    "port": "4567",
    "test_database": {
        "host": "127.0.0.1",
        "port": "6379",
        "password": "",
        "database": "1"
    }
}
```

### 3.3 Run the test suite
- `npm run test`

- How to solve this error?
	- Error message: 
		- `error: NodeBB address in use, exiting... `
		- `Error: listen EADDRINUSE: address already in use 0.0.0.0:4567 `
	- Reason: The port 4567 is used by another process (NodeBB itself in this case)
	- Solution: `./nodebb stop`
	- General Solution (will not work in this case):
		- 1. Find the process ID using port 4567: `sudo lsof -i :4567`
		- 2. Terminate the process: `kill -9 PID` (Replace `PID` with the actual PID)

### 3.4 See full report
- Open the `index.html` file in the `coverage` folder to see the full report

## Step 4: GitHub Issue
### 4.0 Access SonarCloud
- https://sonarcloud.io
- Search for the project and favorite it to see it on your dashboard.
### 4.1 Find a file with warnings
- Navigate to the Issues Tab
- Filter Issues by Criteria

### 4.2 Check existing GitHub Issues
- Navigate to the GitHub Repository, go to the Issues Section, search for Existing Issues. 
	- If issue exists, choose a different issue to work on. 

### 4.3 Create a New GitHub Issue
- Click on "New Issue"
- Fill in the Issue Title, write the Issue Description
- Assign yourself to the Issue

## Step 5: Implement changes

### 5.0 Clone the Repository (if not already done)
- Open terminal. Navigate to desired directory and run: `git clone https://github.com/YourClassRepo.git`

### 5.1 Create a New Branch
- Navigate into the project folder
- It's good practice to work on a separate branch. `git checkout -b refactor-helper-js`

###  5.2 Make Your Changes Locally
- Open the project in your code editor 
- Refactor the code according to the SonarCloud warning and your plan.
- Test Your Changes (linter + test suite)

### 5.3 Commit Changes & Push to Repo
- Stage changes: `git add src/utils/helper.js`
- Commit changes: `git commit -m "Refactored helper.js to reduce cognitive complexity. Fixes #IssueNumber"`
- Push changes to the repository: `git push origin refactor-helper-js`**

### Project Specific Notices
- `public/src` testing: No need to write unit tests, instead should test using front-end interface and developer tools. Add `console.log()` in changed functions, then interact with the interface of `localhost:4567/` and open developer tools to see printed messages in console. 

## Step 6: GitHub Pull Request
### 6.1 Create a Pull Request
- After pushing, navigate back to your GitHub repository page.
- You should see a prompt saying "Compare & pull request". Click on it.
- Fill in Pull Request Details
    - Title: Keep it similar to your commit message.
    - Description: Provide details about what you changed and why.
        - _Include references to the issue number, what was done, and any other relevant information._
- Submit the Pull Request. Click the green "Create pull request" button.

### 6.2 Wait for Review (Optional for this project)
- Your instructors or classmates may review your code.
- Respond to Feedback
	- If reviewers request changes, make them locally, commit, and push them to the same branch. The pull request will update automatically.
- Merge the Pull Request
	- Once approved, click "Merge pull request" and confirm by clicking "Confirm merge".
### 6.3 Close the Issue (Optional for this project)
- After merging, navigate to the **original issue** you created.
- If it isn't closed automatically, click **"Close issue"** button.