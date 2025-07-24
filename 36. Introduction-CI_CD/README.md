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

