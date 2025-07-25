# GitHub Actions, Advanced Concepts and Best Practices.

## Lesson 1: Best Practices for GitHub Actions
**Objectives:**
- Understand how to write maintainable GitHub Actions workflows.
- Learn about code organization and creating modular workflows.

**Lesson Details:**
### Writing Maintainable Workflows:
1. Use Clear and Descriptive Names:
    - Name your workflows, jobs, and steps descriptively for easy understanding.
    - Example: `name: Build and Test Node-js Application`

2. Document Your Workflows:
- Use comment within YAML file to explain the purpose and functionality of complex steps.

### Code Organization and Modular Workflows:
1. Modularize Common Tasks:
- Create reusable workflows or actions for common tasks.
- Use 'uses' to reference other actions or workflows
```bash
jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16.x, 18.x]

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v4
      with:
        node-version: ${{ matrix.node-version }}

    - name: Cache dependencies
      uses: actions/cache@v3
      with:
        path: ~/.npm
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
        restore-keys: |
          ${{ runner.os }}-node-

    - name: Install dependencies
      run: npm ci

    - name: Run Linter
      run: npx eslint .

    - name: Run Unit Tests
      run: npm test

    - name: Build App (optional)
      run: npm run build

```

![](1.%20Steps.png)

2. **Organize Workflow Files:**
- Store workflows in the * -github/workflows* directory.
- Use separate files for different workflows (e.g., ' build.yml*, `deploy-yml`).

## Lesson 2: Performance Optimization
**Objectives:**
- Optimize the execution time of workflows,
- Implement caching to speed up builds.
### Optimizing Workflow Execution Time
1. Parallelize Job:
- Break your workflow into multiple jobs that can run in parallel.
- Use strategy matrix' for testing across multiple environments.
**Coaching Dependencies for Faster Builds:**
1. Implement Caching:
- Use the `actions/cache` action to cache dependencies and build outputs.
```bash
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```
### Lesson 3: Security Consideration
**Objectives:**
- Implement Security best Practices in GitHub Actions
- Secure secret and sensitive information.

**Implementing Security Best Practices:**
1. Least Priviledge Principle:
- Grant minimum permissions necessary for the workflows.
- Regularly review and update permissions.

2. Audit and Monitor Workflow Runs:
-  Regularly check workflow run log for unexpected behaviour.

**Securing Secret and Sentitive Information:**
1. Use Encrypted Secrets:
- Store sensitive Information like tokens and keys in[GitHub Encrypted Secrets](https://docs.github.com/en/actions/how-tos/writing-workflows/choosing-what-your-workflow-does/using-secrets-in-github-actions)
```bash
env:
  ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
# Use secrets as environment variables in your workflow.
```
2. Avoid Hardcoding Sensitive Information:
- Never hardcore sensitive details like Passwords directly in your workflow files.