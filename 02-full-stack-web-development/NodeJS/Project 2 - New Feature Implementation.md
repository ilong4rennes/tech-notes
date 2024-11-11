## Current Architecture of NodeBB
![[Screenshot 2024-11-10 at 11.34.07 PM.png]]
## Integrate Front-end Theme to NodeBB Repo ([link](https://docs.google.com/document/d/17sEb4QTdcKX33vFgN30MWOkPdROW3Z4lzp0KQEhKa6k/edit?usp=sharing))

1. Clone the team's front-end repository in a directory somewhere outside of your NodeBB installation. An ideal place will be in the same parent directory as "NodeBB". This repository contains all the UI templates for development.  Example:

```
/some/dir/NodeBB$ cd ..
/some/dir$ git clone <your_team_frontend_repo>
```

2. Go back to your NodeBB directory and install the custom theme by providing a path to where it is installed. So, you may have to run something like this:

```
/some/dir$ cd NodeBB
/some/dir/NodeBB$ npm install ../<your_frontend_repo_dir>  
```

- Install npm packages using the command:  `npm install <package-name>`
	- It maintains a list of all the dependencies in the `package.json` file.
	- This command will download and install all required libraries into the `node_modules/` folder.

3. Run `./nodebb build tpl` to rebuild the templates. You should not get any errors

- Info: Now, your `NodeBB/package.json` file should have been changed so that the dependency on `nodebb-theme-harmony` no longer points to an NPM version but instead points to `file:<your_frontend_repo_dir>`.

## Implement New Feature from Server Side

The main purpose of this task is to teach us how to navigate through the codebase, and find the right place to implement changes. Personally, I implemented a feature called "Mark Resolved". 
- my [PR link](https://github.com/CMU-313/nodebb-f24-the-turtles/pull/47) | another group's [PR link](https://github.com/CMU-313/nodebb-f24-team-bulbasaur/pull/43)
## Common NodeJS Project Structure (Backend)

1. **`node_modules/`**:
   - **Purpose**: This folder is where **npm** (Node Package Manager) installs all the dependencies (packages or modules) listed in `package.json`. These dependencies can be core libraries, utilities, or any other third-party code required for the project.
   - **Contents**: Contains all third-party libraries. You should never manually edit files in this folder; they are managed by npm.

2. **`src/`:
   - **Purpose**: This folder contains the core source code of your application.
   - **Contents**: You can expect to find server-side code written in JavaScript (or TypeScript). This could include Express.js route handlers, business logic, and utility functions.

3. **`public/`**:
   - **Purpose**: Holds static files like images, stylesheets (CSS), client-side JavaScript, and HTML files that are directly accessible by the client.
   - **Contents**: These files are served directly to the browser in a web application.

4. **`test/`**:
   - **Purpose**: This folder contains automated tests for your application. These tests can verify that individual units of code (unit tests) or entire parts of the application (integration or end-to-end tests) work as expected.
   - **Tools Used**: Often uses testing frameworks like **Mocha**, **Jest**, or **Chai**.

5. **`logs/`**:
   - **Purpose**: Stores log files for server activity. Logs are crucial for debugging and monitoring your application.
   - **Contents**: Files recording errors, access logs, or custom log messages.

6. **`build/`**:
   - **Purpose**: This folder contains compiled or optimized versions of your code. It's often used when you are using TypeScript or bundling front-end assets with Webpack.
   - **Contents**: Bundled JavaScript files, minified CSS files, or transpiled TypeScript files.

7. **`.github/`**:
   - **Purpose**: Stores GitHub-specific settings, like workflows for CI/CD (Continuous Integration/Continuous Deployment).
   - **Contents**: Workflow files for GitHub Actions, templates for issues and pull requests, etc.

8. **`.docker/`**:
    - **Purpose**: Contains files for Docker configuration. Docker helps create consistent development and production environments by containerizing your application.
    - **Contents**: Docker configuration files, including `Dockerfile` and `docker-compose.yml`.

9. **`types/`**:
    - **Purpose**: For TypeScript projects, this folder holds the type definitions, either for your own code or for third-party libraries.
    - **Contents**: Files ending with `.d.ts`, providing TypeScript with type definitions.

10. **`install/`**:
    - **Purpose**: It might contain installation scripts or post-install configurations for setting up the application after cloning it from the repository.
    - **Contents**: Bash scripts or npm hook scripts that are run post-install.

11. **`.editorconfig`**:
    - **Purpose**: Defines and maintains consistent coding styles between different editors and IDEs (e.g., indentation, line endings).
    - **Contents**: Configurations for code style preferences.

12. **`.eslintrc`, `.eslintignore`, `.eslintcache`**:
    - **Purpose**: Configuration files for **ESLint**, a tool that checks JavaScript code for errors and enforces coding standards.
    - **Contents**: Rules for linting and ignoring certain files or directories during linting.

13. **`.gitignore`**:
    - **Purpose**: Specifies files or directories that should not be committed to Git.
    - **Contents**: Typically excludes `node_modules/`, log files, or environment configuration files like `.env`.

14. **`.dockerignore`**:
    - **Purpose**: Similar to `.gitignore`, but specifies files and folders that should be ignored when building Docker images.

15. **`app.js` or `index.js`**:
    - **Purpose**: This is often the **entry point** for the application. It initializes the server, imports necessary modules, and sets up routes and middleware.
    - **Contents**: Typically contains code to start the Express server, set up middleware, and import routers.

