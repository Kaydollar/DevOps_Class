# Inroduction to CI/CD

STEP 1: Initialize a GITHUB Repository
1. Create a new folder:
```bash
mkdir my-node-app && cd my-node-app
```
![](1.%20mkdir.png)
2. Initialize Git:
```bash
git init
```
3. Create a .gitignore file:
```bash
npx gitignore node
```
![](2.%20gitignore.png)
4. Push to GitHub:
```bash
git remote add origin https://github.com/Kaydollar/my-node-app.git
git add .
git commit -m "Initial commit"
git push -u origin master
```
![](3.%20Git%20Push.png)

Step 2:
1. Initialize Node.js project:
```bash
npm init -y
```
![](4.%20npm%20init.png)
2. Create a basic app:
- Create a file for your code, e.g., index.js:
```bash
nano index.js
```
- Paste your Node.js code into the file:
```bash
const express = require('express');
const app = express();
const port = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  console.log(`App listening at http://localhost:${port}`);
});
```
3. Save and exit:

Press `CTRL+O` to save

Press `ENTER` to confirm

Press `CTRL+X` to exit

**Prerequisites:**
- Make sure express is installed:
```bash
npm install express
```
![](5.%20install%20Express.png)
- Make sure Node.js is installed:
```bash
node -v
```
![](6.%20node.png)

4. Run the app using Node.js:
```bash
node index.js
```
![](7.%20App%20Listening.png)

4. Add a start script in package.json:
```bash
"scripts": {
  "start": "node index.js"
}
```
- Open your package.json file:
```bash
nano package.json
```
- Look for the "scripts" section. It might look like this:
```bash
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1"
},
```
- Replace or modify it like this:
```bash
"scripts": {
  "start": "node index.js",
  "test": "jest"
},
```
- Save and exit:

Press `CTRL + O`, then ENTER to save

Press `CTRL + X` to exit

**To start your app:**
```
npm start
```

**To run tests (once Jest is installed):**
```
npm test
```
Step 3: Writing Your First GitHub Action Workflow
1. Create folders:
```bash
mkdir -p .github/workflows
```
2. Create the CI workflow file:
```bash
nano .github/workflows/ci.yml
```
- Paste This YAML into ci.yml
```bash
name: Node.js CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout source code
      uses: actions/checkout@v4

    - name: Set up Node.js
      uses: actions/setup-node@v4
      with:
        node-version: 18

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test
```
- Commit and Push It
Once you've created and saved ci.yml, commit it and push to GitHub:
```bash
git add .github/workflows/ci.yml
git commit -m "Add GitHub Actions CI workflow"
git push origin main
```
**View Workflow Runs**
- Go to your repo on GitHub.

- Click the "Actions" tab.

- You’ll see your workflow running automatically.

Step 4: Testing and Deployment

**Add automated tests**
1. Install Jest:
```bash
npm install --save-dev jest
```
![](8.%20Install.png)
2. Add a sample test:

create app.test.js file

Add file 
```bash
test('2 + 2 equals 4', () => {
  expect(2 + 2).toBe(4);
});
```
3. Run tests locally:
```bash
npm test
```
![](9.%20test.png)

**Deployment to Heroku (via GitHub Actions)**
We’ll use a GitHub Actions step to auto-deploy the app to Heroku after tests pass.
1. Get Your Heroku API Key
```bash
heroku login
heroku apps:create my-node-ci-app
heroku auth:token
```
Pre-requisite:

**Install Heroku CLI on Ubuntu (EC2)**
```bash
curl https://cli-assets.heroku.com/install.sh | sh
```
![](10.%20Install%20heroku.png)
**This script will:**
- Download the Heroku CLI

- Install it to /usr/local/lib/heroku

- Add it to your PATH

**After Installing, Verify:**
```bash
heroku --version
```
![](11.%20heroku%20version.png)
**Then Run Your Commands:**

Copy the output from heroku auth:token and save it — you’ll use it in your GitHub repo secrets as HEROKU_API_KEY.

![](12.%20Secret%20Code.png)

**Update GitHub Actions Workflow for Deploy**

`Edit .github/workflows/ci.yml and add the deploy job:`

```bash
name: Node.js CI/CD

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Set up Node.js
      uses: actions/setup-node@v4
      with:
        node-version: 18

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

  deploy:
    needs: build
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v4

    - name: Deploy to Heroku
      uses: akhileshns/heroku-deploy@v3.12.12
      with:
        heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
        heroku_app_name: ${{ secrets.HEROKU_APP_NAME }}
        heroku_email: ${{ secrets.HEROKU_EMAIL }}
```
Commit and push:

```bash
git add .
git commit -m "Add test and deploy to Heroku"
git push origin main
```
![](13.%20git%20push.png)

Go to GitHub > Actions to watch it run:
- Test phase should pass

- Deploy phase will push to Heroku

**Enhanced Test Cases (Jest Example)**
```bash
// __tests__/app.test.js
const request = require('supertest');
const app = require('../index'); // or wherever your Express app is exported

describe('GET /', () => {
  it('should respond with 200 and expected message', async () => {
    const response = await request(app).get('/');
    expect(response.statusCode).toBe(200);
    expect(response.text).toMatch(/Hello|Welcome|OK/); // Customize to your app
  });
});
```
Run with:
```bash
npm install --save-dev jest supertest
```
![](14.%20Express.png)
**Update package.json:**
```bash
"scripts": {
  "test": "jest"
}
```
**Use a Build Matrix in GitHub Actions**

Test your app with multiple Node.js versions for better compatibility:

```bash
name: Node.js CI/CD

on:
  push:
    branches: [ "main", "dev", "staging" ]
  pull_request:
    branches: [ "*" ]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16, 18, 20]  # You can add/remove versions here

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```
**Add Dynamic Branch Deploy & Error Handling**

Update the deploy section to:
- Deploy only from `main`
- Fail gracefully with useful logs
- Add retry logic (optional)

```bash
  deploy:
    needs: build
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to Heroku (if branch is main)
        uses: akhileshns/heroku-deploy@v3.12.12
        with:
          heroku_api_key: ${{ secrets.HEROKU_API_KEY }}
          heroku_app_name: ${{ secrets.HEROKU_APP_NAME }}
          heroku_email: ${{ secrets.HEROKU_EMAIL }}
        continue-on-error: false  # Force exit if deploy fails

      - name: Notify failure (optional)
        if: failure()
        run: echo "🚨 Deployment failed!"
```
**Configuring Build Matrices**

- Implement matrix builds to test across multiple version or environment.
- Mange build dependecies efficiently.
1. Parallel and Matrix Build:
    
    A matrix build allows you to run job across multiple environments and versions simultaneously, increasing efficiency.

    This is useful for testing your application in different versions of runtime environment or dependencies. 

```bash
strategy:
  matrix:
    node-version: [12.x, 14.x, 16.x]
    # This matrix will run the job multiple times, once for each specified Node.js version (12.x, 14.x, 16.x).
    # The job will be executed separately for each version, ensuring compatibility across these versions.
```
2. Manage Build Dependencies:

    Handling dependencies and services required for your Build process is crucial.

    Utilizing caching to reduce the time spent on downloading and installing dependencies repeatedly.
```bash
- name: Cache Node Modules
  uses: actions/cache@v2
  with:
    path: ~/.npm
    key: ${{" runner.os "}}-node-${{" hashFiles('**/package-lock.json') "}}
    restore-keys: |
      ${{" runner.os "}}-node-
  # This snippet caches the installed node modules based on the hash of the 'package-lock.json' file.
  # It helps in speeding up the installation process by reusing the cached modules when the 'package-lock.json' file hasn't changed.
```

**Full Example: Matrix + Cache**
```bash
name: Node.js Matrix CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "*" ]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x, 14.x, 16.x]

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Cache Node modules
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test
```
![](15.%20Multiple.png)

**Integrating Code Quality Checks**

- Integrate code Analysis tools into the GitHub Actions Workflows
- Configure linters and static code analyzers for maintaining code quality. 

1. Adding code Analysis tools:
- Include steps in your workflow to run tools that Analyse code quality and adherence to coding standards.

```bash
- name: Run Linter
  run: npx eslint .
  # 'npx eslint .' runs the ESLint tool on all the files in your repository.
  # ESLint is a static code analysis tool used to identify problematic patterns in JavaScript code.
```
2. Configuring Linters and Static code Analyzers:
- Ensure your repositories includes configuration files for this tools such as `eslintrc` for ESLint.
```bash
# Ensure to include a .eslintrc file in your repository
# This file configures the rules for ESLint, specifying what should be checked.
# Example .eslintrc content:
# {"\n   #   \"extends\": \"eslint:recommended\",\n   #   \"rules\": {\n   #     // additional, custom rules here\n   #   "}
# }
```
**Running ESLint in GitHub Actions Workflow**

Add this step after installing dependencies, so the linter can analyze your code:
```bash
- name: Run Linter
  run: npx eslint .
```

 **Sample .eslintrc.json Configuration**
- You need this file in your root directory to define your rules:
```bash
{
  "env": {
    "browser": true,
    "es2021": true,
    "node": true
  },
  "extends": "eslint:recommended",
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "rules": {
    "no-unused-vars": "warn",
    "no-console": "off",
    "semi": ["error", "always"]
  }
}
```
![](16.%20eslintrc.png)
**Complete Workflow: CI + Lint + Matrix**
- Here's a complete workflow integrating all the pieces:
```bash
name: Node.js CI/CD with Linting

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "*" ]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [14.x, 16.x]

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set up Node.js ${{ matrix.node-version }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}

      - name: Cache Node modules
        uses: actions/cache@v3
        with:
          path: ~/.npm
          key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
          restore-keys: |
            ${{ runner.os }}-node-

      - name: Install dependencies
        run: npm ci

      - name: Run ESLint
        run: npx eslint .

      - name: Run tests
        run: npm test
```
![](17.%20nano.png)
