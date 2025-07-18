# Introduction to Continuous Integration and Continuous Development.

The project will involve setting up a simple web application (e.g., a Node.js application) and applying CI/CD practices using GitHub Actions. This application will have basic functionality, such as serving a static web page.

**Introduction to GitHub Actions and CI/ CD Course Project**

Welcome to our course on GitHub Actions and Continuous Integration/ Continuous Deployment (CI/CD).
This course is designed to provide a hands-on learning experience, guiding you through the essentials of automating software development processes using GitHub Actions. Whether you're a developer, a student, or just curious about CI/CD practices, this course will equip you with the practical skills and knowledge you need to implement these powerful automation techniques in your projects.

**Why is this relevant to learner**

Imagine you're a chef in a busy restaurant. Every dish you prepare is like a piece of software code.
Without a systematic approach, you might end up with orders being mixed up, dishes taking too long to prepare, or worse, the quality of the food being inconsistent. This is where a well-organized kitchen, with clear processes and automation (like having appliances that precisely time and cook parts of the dishes). comes into play. In software development, CI/CD is akin to this efficient kitchen. It ensures that your dishes' (software builds) are consistently 'cooked' (built, tested, and deployed) with precision and efficiency. By learning GitHub Actions and CI/CD, you're essentially learning how to set up and manage your high-tech kitchen in the software world, allowing you to serve dishes faster, with higher quality, and with fewer 'kitchen mishaps' (bugs and deployment issues).

This course will help you understand and implement these practices, making your software development process more efficient and error-free, much like a well-orchestrated kitchen. Whether you're working on personal projects, contributing to open source, or building enterprise-level applications, mastering CI/CD| with GitHub Actions will be an invaluable skill in your development toolkit.

**Pre-requisites**

1. Basic Knowledge of Git and GitHub:
    - Understanding of version control concepts.

    - Familiarity with basic Git operations like clone, commit, push, and pull.

    - A GitHub account and knowledge of repository management on GitHub.

2. Understanding of Basic Programming Concepts:

    - Fundamental programming knowledge, preferably in JavaScript, as the example project uses Node.js.

    - Basic understanding of how web applications work.

3. Familiarity with Node.js and npm:

    - Basic knowledge of Node.js and npm (Node Package Manager).

    -  Ability to set up a simple Nodejs project and install dependencies using pm.

4. Text Editor or IDE:

    - A text editor or Integrated Development Environment (IDE) like Visual Studio Code, Atom, Sublime Text, or any preferred editor for writing and editing code.

5. Local Development Environment:

    - Node.js and pm installed on the local machine.

    - Access to the command line or terminal.

6. Internet Connection:

    - Stable internet connection to access GitHub and potentially other online resources or documentation.

7. Basic Understanding of CI/CD Concepts (Optional but Helpful):

    - General awareness of Continuous Integration and Continuous Deployment concepts.

    - This can be part of the learning in the course, but prior knowledge is beneficial.

### Lesson 1: Understanding Continuous Integration and Continuous Deployment

## Objectives:

1. Define CI/CD and understand its benefits.

2. Get familiar with the CI/CD pipeline.

## Lesson Details:

**1. Definition and Benefits of CI/CD:**

- Continuous Integration (CI) is the practice of merging all developers' working copies to a shared mainline several times a day.

- Continuous Deployment (CD) is the process of releasing software changes to production automatically and reliably.

- **Benefits:** Faster release rate, improved developer productivity, better code quality, and enhanced customer satisfaction.

### 2. Overview of the CI/CD Pipeline:

-  **CI Pipeline** typically includes steps like version control, code integration, automated testing, and |
building the application.

- **CD Pipeline** involves steps like deploying the application to a staging or production environment, and post-deployment monitoring.

- **Tools:** Version control systems (e.g., Git), CI/CD platforms (e.g., GitHub Actions), testing frameworks, and deployment tools.

## Lesson 2: Introduction to Github Actions

**Objectives**

1. Understand what GitHub Action is.

2. Learn Key Concepts and terminology

**Lesson Details**

GitHub Actions: A CI/CD 

- **GitHub Actions:** A CI/CD platform integrated into GitHub, automating the build, test, and deployment pipelines of your software directly within your GitHub repository.

- **Documentation Reference:** Explore the [GitHub Actions Documentation](https://docs.github.com/en/actions) for in-depth understanding.

Key Concepts and Terminology:
1. **Workflow:**

- **Definition:** A configurable automated process made up of one or more jobs. Workflows are defined by a YAML file in your repository.

- **Example:** A workflow to test and deploy a Nodejs application upon a Git push.

- **Documentation:** Learn more about workflows in the [GitHub Docs on Workflows](https://docs.github.com/en/actions/how-tos/writing-workflows)

2. Event: 

- **Definition:** A specific activity that triggers a workflow. Events include activities like push, pull request, issue creation, or even a scheduled time.

- **Example:** A `push` event triggers a workflow that runs tests every time code is pushed to any branch in a repository.

- **Documentation:** Review different types of events in the [Events that trigger workflows section](https://docs.github.com/en/actions/reference/events-that-trigger-workflows).

3. **Job:**

- **Definition:** A set of steps in a workflow that are executed on the same runner. Jobs can run
sequentially or in parallel.

- **Example:** A job that runs tests on your application.

- **Documentation:** Understand jobs in detail in the [GitHub Docs on Jobs](https://docs.github.com/en/actions/how-tos/writing-workflows/choosing-what-your-workflow-does).

4. **Step:**

• Definition: An individual task that can run commands within a job. Steps can run scripts or actions.
Example: A step in a job to install dependencies (`npm install`).
Documentation: Learn about steps in the [Steps section of GitHub Docs](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idsteps).
5. **Action:**

- **Definition:** Standalone commands combined into steps to create a job. Actions can be written by
you or provided by the GitHub community.

- **Example:** Using `actions/checkout` to check out your repository code.

Documentation: Explore GitHub Actions in the [Marketplace](https://github.com/marketplace?type=actions) and learn how to create your own in the [Creating actions guide](https://docs.github.com/en/actions/how-tos/sharing-automations).

6. Runner:

- **Definition:** A server that runs your workflows when they're triggered. Runners can be hosted by GitHub or self-hosted.

- **Example:** A GitHub-hosted runner that uses Ubuntu.

- **Documentation:** Delve into runners in the [GitHub Docs on Runners](https://docs.github.com/en/actions/how-tos/using-github-hosted-runners).

**Additional Resources:**

**GitHub Learning Lab:** Interactive courses to learn GitHub Actions. Visit [GitHub Learning Lab](https://lab.github.com/courses)

**GitHub Actions Quickstart:** For a hands-on introduction, check out the [Quickstart for GitHub Actions](https://docs.github.com/en/actions/get-started/quickstart).

- **Community Forums:** Engage with the GitHub community for questions and discussions at [GitHub
Community Forums](https://github.com/orgs/community/discussions/).

# Practical Implementation

Setting Up the Project:

1. **Initialize a GitHub Repository:**

- Create a new repository on GitHub.

![](1.%20Create%20new%20Repo.png)

- Clone it to your local machine.

![](2.%20Git%20Clone.png)

2. **Create a Simple Node.js Application:**

- Initialize a Node.js project ("npm init*).

![](3.%20npm%20init.png)

This creates a package.json file with default values.

- Create a simple server using Express.js to serve a static web page.

![](4.%20Express%20JS.png)

- Add your code to the repository and push it to GitHub.

```bash
// Example: index.js
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => {"\n     res.send('Hello World!');\n   "});

app.listen(port, () => {
  console.log(`App listening at http://localhost:${port}`);
});
```
![](5.%20Git%20Push.png)

3. **Writing Your First GitHub Action Workflow:**

- Create a `.github/workflow` directory in your Repository.

- Add a workflow file (e.g `node.js.yml`).

```bash
# Example: .github/workflows/node.js.yml

# Name of the workflow
name: Node.js CI

# Specifies when the workflow should be triggered
on:
# Triggers the workflow on 'push' events to the 'main' branch
push:
    branches: [ main ]
# Also triggers the workflow on 'pull_request' events targeting the 'main' branch
pull_request:
    branches: [ main ]

# Defines the jobs that the workflow will execute
jobs:
# Job identifier, can be any name (here it's 'build')
build:
    # Specifies the type of virtual host environment (runner) to use
    runs-on: ubuntu-latest

    # Strategy for running the jobs - this section is useful for testing across multiple environments
    strategy:
    # A matrix build strategy to test against multiple versions of Node.js
    matrix:
        node-version: [14.x, 16.x]

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
    - # Checks-out your repository under $GITHUB_WORKSPACE, so the job can access it
    uses: actions/checkout@v2

    - # Sets up the specified version of Node.js
    name: Use Node.js ${{" matrix.node-version "}}
    uses: actions/setup-node@v1
    with:
        node-version: ${{" matrix.node-version "}}

    - # Installs node modules as specified in the project's package-lock.json
    run: npm ci

    - # This command will only run if a build script is defined in the package.json
    run: npm run build --if-present

    - # Runs tests as defined in the project's package.json
    run: npm test
```
![](6.%20workflow.png)

### Explanation:

1. `name`: This simply names your workflow. It's what appears on GitHub when the workflow is running.

2. `on`: This section defines when the workflow is triggered. Here, it's set to activate on push and pull request events to the main branch.

3. `jobs`: Jobs are a set of steps that execute on the same runner. In this example, there's one job named `build`

4. `runs-on`: Defines the type of machine to run the job on. Here, it's using the latest Ubuntu virtual machine.

5. `strategy-matrix`: This allows you to run the job on multiple versions of Node.js, ensuring compatibility.

6. `steps`: A sequence of tasks executed as part of the job.

- `actions/checkout@v2`: Checks out your repository under `$GITHUB WORKSPACE`.

- `actions/setup-node@v1`: Sets up the Node.js environment.

- `npm ci`: Installs dependencies defined in `package-lock-json`.

- `opm run build --if-present`: Runs the build script from " package-json if it's present.

- `npm test`: Runs tests specified in `package.json`.

This workflow is a basic example for a Nodejs project, demonstrating how to automate testing across different Nodejs versions and ensuring that your code integrates and works as expected in a clean environment.

4. Testing and Deployment:

- Add automated tests for your application.

- Create a workflow for deployment (e.g., to a cloud service like Heroku or AWS).

5. Experiment and Learn

- Modify workflow to see how changes affects the CI/CD Process

- Try adding different types of tests (unit test, integration test). 