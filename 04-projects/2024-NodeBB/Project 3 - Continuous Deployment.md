---
created: "2024"
tags:
  - "#CICD"
  - "#deployment"
  - "#github-actions"
org:
  - CMU
goal: Deploy to Azure using GitHub Actions
---
## Step 1: Deployment on Azure with GitHub Actions ([link](https://docs.google.com/document/d/1WXRbEXMJf_9IIAsbYFqjfkUsnAXJk6NQ0v7OVDIBClM/edit?usp=sharing))

## Step 2: Add the Workflow File

### Key Concepts

1. **Workflow**: 
   - A workflow is a set of automated steps defined in a YAML file that is executed when a specific event occurs in the repository (e.g., pushing code, creating a pull request, or on a schedule).
   - Workflows are stored in the `.github/workflows` directory of your GitHub repository.

2. **Jobs**: 
   - Each workflow can have one or more **jobs**. A job is a series of steps that are executed on the same runner (a virtual machine).
   - Jobs can run in parallel or have dependencies on each other (e.g., one job waits for another to finish).

3. **Steps**: 
   - **Steps** are the individual actions or commands that are executed within a job.
   - Each step can run a shell command or use a pre-built action, like checking out code from the repository, setting up a programming language environment, or deploying code.

4. **Actions**: 
   - **Actions** are reusable components that perform specific tasks in a workflow, like installing dependencies or deploying to a cloud service.
   - You can use actions created by the community or create your own.

5. **Events**: 
   - Workflows are triggered by **events** such as `push`, `pull_request`, `schedule`, `workflow_dispatch` (manual trigger), and more.
   - You specify the event that will trigger the workflow in the `on` section of the YAML file.

6. **Runners**: 
   - A **runner** is a server that executes your workflow. GitHub provides hosted runners for popular operating systems, or you can set up self-hosted runners.

### Sample Structure of a Workflow File

```yaml
# .github/workflows/example-workflow.yml

name: Example Workflow  # Name of the workflow

on:
  push:  # Trigger workflow on push
    branches:
      - main  # Specify which branches trigger the workflow
  pull_request:  # Trigger on pull request events

jobs:
  job1:  # Name of the first job
    runs-on: ubuntu-latest  # Specify the environment (OS) for the job

    steps:  # Steps to execute in this job
      - name: Check out code  # Step description
        uses: actions/checkout@v4  # Use a pre-built action to check out the code

      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '16'  # Specify the Node.js version

      - name: Install dependencies
        run: npm install  # Run a shell command to install dependencies

      - name: Run tests
        run: npm test  # Run a shell command to execute tests

  job2:  # Name of the second job (if needed)
    runs-on: ubuntu-latest
    needs: job1  # This job will run only after job1 completes successfully

    steps:
      - name: Deploy to Production
        run: echo "Deploying to production..."  # Replace with your deployment commands
```
### Components Explained

1. **name**: Gives the workflow a name for easy identification.
2. **on**: Defines the events that trigger the workflow. Examples include:
   - `push`: Runs the workflow when code is pushed to the specified branches.
   - `pull_request`: Runs the workflow when a pull request is opened or updated.
   - `workflow_dispatch`: Allows you to manually trigger the workflow from the GitHub Actions interface.
3. **jobs**: Lists all the jobs in the workflow.
   - Each job runs independently, but you can make them dependent on each other using `needs`.
4. **runs-on**: Specifies the environment for the job (e.g., `ubuntu-latest`, `windows-latest`, `macos-latest`).
5. **steps**: Contains the individual steps for a job. Each step can:
   - Use a pre-built action from the GitHub Marketplace (e.g., `actions/checkout@v4`).
   - Run a shell command using `run`.

---
### Workflow file for our project

```yaml
# Docs for the Azure Web Apps Deploy action: https://github.com/Azure/webapps-deploy
# More GitHub Actions for Azure: https://github.com/Azure/actions

name: Build and deploy Node.js app to Azure Web App - nodebb-f24

on:
  push:
    branches:
      - f24
  workflow_dispatch:
    
concurrency:
  group: ${{ github.workflow }}
  cancel-in-progress: true

jobs:
  lint-and-test:
    uses:
      ./.github/workflows/test.yaml

  build-and-deploy:
    if: github.repository == 'cmu-313/nodebb-f24-the-turtles'
    needs: lint-and-test

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up Node.js version
        uses: actions/setup-node@v3
        with:
          node-version: '20.17.0'

      - name: Install Packages
        run: npm install
        
      - name: Set up NodeBB
        run: |
          ./nodebb setup '{"url":"https://nodebb-team-turtles.azurewebsites.net:443",
            "admin:username": "admin",
            "admin:password": "${{ secrets.ADMIN_PASSWORD }}",
            "admin:password:confirm": "${{ secrets.ADMIN_PASSWORD }}",
            "admin:email": "rohanpadhye@cmu.edu",
            "database": "redis",
            "redis:host": "${{ secrets.REDIS_HOST }}",
            "redis:port": "6379",
            "redis:password": "${{ secrets.REDIS_PASSWORD }}" }'
      
      - name: Set up Frontend
        run: |
          npm install https://github.com/CMU-313/nodebb-frontend-f24-the-turtles.git
      
      - name: Rebuild
        run: |
          ./nodebb build
          
      - name: 'Deploy to Azure Web App'
        id: deploy-to-webapp
        uses: azure/webapps-deploy@v2
        with:
          app-name: 'nodebb-team-turtles'
          slot-name: 'Production'
          publish-profile: ${{ secrets.AZUREAPPSERVICE_PUBLISHPROFILE_7AE551742B954B0DAD0B614473255649 }}
          package: .

```

### Example Workflows in Our Project

1. **`azure-deploy-f24.yml`**: This workflow is responsible for deploying your Node.js app to Azure. It likely includes steps for setting up the Node.js environment, building the app, and using an Azure action to deploy.
2. **`docker.yml`**: A workflow that may be used for building and testing your app in a Docker container. Useful for containerized applications.
3. **`jshint.yml`**: This workflow is used for running JSHint, a static analysis tool for JavaScript, to catch syntax errors and issues in your code.
4. **`test.yaml`**: Contains the steps for running automated tests, ensuring your code works as expected.
5. **`volunteers.yaml`**: This could be a workflow for managing tasks related to volunteers or specific processes for contributors.
