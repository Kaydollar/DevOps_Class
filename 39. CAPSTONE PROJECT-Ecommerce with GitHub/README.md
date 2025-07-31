# CAPSTONE PROJECT- Ecommerce Application CI/CD Pipeline.

**Project Overview:** Automated Pipeline for an E-commerce Platform

**Hypothetical Use Case:**

Tasked with developing and maintaining an e-commerce platform. The Platform has two primary components. 

- E-commerce API: Backend Service handling product listings, user account and order processing.
- E-commerce Frontend: A web Application for users to browse products, manage their accounts, and place orders.

The goal is to Automate the integration and deployment process for both components using GitHub Actions, ensuring continuous delivery and Integration.

## Project Tasks:
### TASK 1: Project Setup
- Create a new GitHub repository named `ecommerce-platform`

![](1.%20Create%20Repo.png)

![](2.%20Git%20clone.png)

- Inside the repository, create two directories: `api` for the backend and `webapp` for the front end.

![](3.%20mkdir.png)

**Push structure to GitHub:**
```bash
git add .
git commit -m "Initial project structure with api and webapp folders"
git push origin main
```
![](4.%20Push.png)
### TASK 2: Initialize GitHub Actions
- Initialize a GIT repository and add your initial project structure.

**In the root of the repo:**
```bash
mkdir -p .github/workflows
```
- Create `.gitHub/workflows` directory in your repository for GitHub Actions.
- **This is where your CI/CD YAML files will go.**
### TASK 3: Backend API Setup
- In the `api` directory, setup a simple Node.js/Express application that handles basic e-commerce operations.

**Step-by-Step:**
1. Navigate to the api folder and initialize Node:
```bash
cd api
npm init -y
```
![](5.%20api.png)

2. Install dependencies:
```bash
npm install express
npm install --save-dev jest supertest
```
![](6.%20npm%20install.png)
3. Create a basic Express app in index.js:
```bash
const express = require('express');
const app = express();
app.get('/', (req, res) => res.send('E-commerce API Running'));
module.exports = app;
if (require.main === module) app.listen(3000);
```
4. Create a test in test/app.test.js:
```bash
const request = require('supertest');
const app = require('../index');
test('GET / should return API status', async () => {
  const res = await request(app).get('/');
  expect(res.text).toBe('E-commerce API Running');
});
```
5. Add test script to package.json:
```bash
"scripts": {
  "test": "jest"
}
```
- Implement unit tests for API
### TASK 4: Frontend Web Application Setup

**Step-by-Step:**
1. Use Create React App in the webapp folder:
```bash
cd ../webapp
npx create-react-app .
```
- In the `webapp` directory, create a simple react application that interacts with the Backend API.

![](7.%20react.png)

![](8.%20axios.png)

- Ensure the Frontend has basic features like product listing, user login, and order placement.

### TASK 5: Continuous Integration Workflow
- With A GitHub Actions workflow for the backend and frontend that:
- Install dependencies
- Run tests
- Build the Application.

1. Backend CI Workflow (api)
- Create a file named:
```bash
.github/workflows/backend-ci.yml
```
Sample backend-ci.yml
```bash
name: Backend CI

on:
  push:
    paths:
      - 'api/**'      # Only run when code in 'api' changes
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: api   # Set the working directory to 'api'

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: 18

    - name: Cache node modules
      uses: actions/cache@v3
      with:
        path: ~/.npm
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
        restore-keys: ${{ runner.os }}-node-

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test
```
2. Frontend CI Workflow (webapp)
- Create another file named:
```bash
.github/workflows/frontend-ci.yml
```
Sample frontend-ci.yml
```bash
name: Frontend CI

on:
  push:
    paths:
      - 'webapp/**'
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: webapp

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Node.js
      uses: actions/setup-node@v3
      with:
        node-version: 18

    - name: Cache node modules
      uses: actions/cache@v3
      with:
        path: ~/.npm
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
        restore-keys: ${{ runner.os }}-node-

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

    - name: Build React app
      run: npm run build
```
Here's how your folder structure should look:

```bash
ecommerce-platform/
├── api/
│   └── ...        # Your backend code (Node.js/Express)
├── webapp/
│   └── ...        # Your frontend code (React)
└── .github/
    └── workflows/
        ├── backend-ci.yml
        └── frontend-ci.yml
```

### TASK 6: Docker Integration:
- Create Dockerfile for both the Backend and Frontend
- Modify your GitHub Actions workflow to build Docker Images.

 In each folder (api and webapp), create a Dockerfile.
- Step 1: Create Dockerfile for the Backend (api/)
Create this file: ecommerce-platform/api/Dockerfile
```bash
# Base image
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of the app
COPY . .

# Expose port (if your API runs on 5000)
EXPOSE 5000

# Start the application
CMD ["npm", "start"]
```
- Step 2: Create Dockerfile for the Frontend (webapp/)

Create Dockerfile for the Frontend (webapp/)
```bash
# Base image for build
FROM node:18-alpine as build

WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Production image
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
- Step 3: Update GitHub Actions to Build Docker Images
In .github/workflows/backend-ci.yml, add:
```bash
- name: Build Docker Image - Backend
  run: docker build -t ecommerce-api ./api
```

In .github/workflows/frontend-ci.yml, add:
```bash
- name: Build Docker Image - Frontend
  run: docker build -t ecommerce-frontend ./webapp
```
### TASK 7: Deploy to cloud
- Choose a cloud Platform for Deployment(AWS)
- Configure GitHub Actions to deploy the Docker Images to the chosen cloud Platform.
- Use GitHub secrets to securely store and access cloud credentials

- Step 1: Set Up AWS Infrastructure (One-Time)

Use AWS Console
1. Go to Amazon ECR > Create Repository
* Name: `ecommerce-api` and `ecommerce-frontend`
* Make them public or private as needed

2. Go to Amazon ECS > Create Cluster
* Select Fargate (Serverless)

3. Define two services:
* API Service → Uses `ecommerce-api` image
* Frontend Service → Uses `ecommerce-frontend` image
* Configure port mappings (e.g., 5000 and 80)

Open your terminal in the root of your project and create the file:
- Create docker-compose.yml
```bash
version: '3.8'

services:
  api:
    image: ecommerce-api
    build:
      context: ./api
    ports:
      - "5000:5000"
    environment:
      - NODE_ENV=production
    restart: always

  frontend:
    image: ecommerce-frontend
    build:
      context: ./webapp
    ports:
      - "80:80"
    environment:
      - NODE_ENV=production
    restart: always
```
Step 2: GitHub Action — Build and Push Docker Images to ECR

Add this workflow file: `.github/workflows/deploy-aws.yml`
```bash
name: Deploy to AWS
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v2

    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}

    - name: Log in to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v1

    - name: Build, Tag, and Push Backend (API)
      run: |
        docker build -t ecommerce-api ./api
        docker tag ecommerce-api:latest ${{ steps.login-ecr.outputs.registry }}/ecommerce-api:latest
        docker push ${{ steps.login-ecr.outputs.registry }}/ecommerce-api:latest

    - name: Build, Tag, and Push Frontend
      run: |
        docker build -t ecommerce-frontend ./webapp
        docker tag ecommerce-frontend:latest ${{ steps.login-ecr.outputs.registry }}/ecommerce-frontend:latest
        docker push ${{ steps.login-ecr.outputs.registry }}/ecommerce-frontend:latest
```
Step 3: Auto-Deploy to ECS

Manual Setup (simpler for now)

Once the images are in ECR:
- Go to ECS > Your Cluster > Services
- Edit service > Update image URI
- Click Deploy

Step 4: Add GitHub Secrets
Go to your GitHub repo → Settings > Secrets and Variables > Actions

Add:
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- AWS_REGION (e.g., us-east-1)

### TASK 8: Continuous Deployment
- Configure your workflow to deloy updates automatically to the cloud environment when changes are pushed to the main branch.

Step 1: Store AWS Credentials Securely
In your GitHub repo:

Go to Settings > Secrets and Variables > Actions

Add the following secrets:

AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

AWS_REGION

ECR_REPOSITORY_API

ECR_REPOSITORY_FRONTEND

ECS_CLUSTER_NAME

ECS_SERVICE_NAME_API

ECS_SERVICE_NAME_FRONTEND

TASK_DEFINITION_API

TASK_DEFINITION_FRONTEND

**Step 2:** Create the Deployment Workflow

Create this file: `.github/workflows/deploy.yml`
```bash
name: CI/CD Pipeline

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v2
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}

    - name: Log in to Amazon ECR
      run: |
        aws ecr-public get-login-password --region ${{ secrets.AWS_REGION }} |
          docker login --username AWS --password-stdin public.ecr.aws

    - name: Build and Push API Image
      run: |
        cd api
        docker build -t ${{ secrets.ECR_REPOSITORY_API }} .
        docker tag ${{ secrets.ECR_REPOSITORY_API }}:latest public.ecr.aws/${{ secrets.ECR_REPOSITORY_API }}:latest
        docker push public.ecr.aws/${{ secrets.ECR_REPOSITORY_API }}:latest

    - name: Build and Push Frontend Image
      run: |
        cd ../webapp
        docker build -t ${{ secrets.ECR_REPOSITORY_FRONTEND }} .
        docker tag ${{ secrets.ECR_REPOSITORY_FRONTEND }}:latest public.ecr.aws/${{ secrets.ECR_REPOSITORY_FRONTEND }}:latest
        docker push public.ecr.aws/${{ secrets.ECR_REPOSITORY_FRONTEND }}:latest

    - name: Update ECS Services
      run: |
        aws ecs update-service --cluster ${{ secrets.ECS_CLUSTER_NAME }} \
          --service ${{ secrets.ECS_SERVICE_NAME_API }} \
          --force-new-deployment

        aws ecs update-service --cluster ${{ secrets.ECS_CLUSTER_NAME }} \
          --service ${{ secrets.ECS_SERVICE_NAME_FRONTEND }} \
          --force-new-deployment
```

### TASK 9: Performance and Security
- Implement caching in your Workflow to optimize build times
- Ensure all sensitive data, Including API keys and database credentials, are secured using GitHub secrets.

1. Caching Dependencies

✅ Node.js Projects (Backend + Frontend)

Add these steps to your GitHub workflow before installing dependencies:
```bash
- name: Cache Node modules
  uses: actions/cache@v3
  with:
    path: |
      ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```


### TASK 10: Project Documentation 
- Document your Project Setup, workflow details, and Instructions for local development in a `README.md` file.

### Conclusion:
This capstone project aims to provide hands-on experience in automating CI/CD pipelines for a real-world e-commerce application, encompassing aspects like backend API management, frontend web development, Docker containerization, and cloud deployment.

**Additional Resources:**
- [Node.js](https://nodejs.org/docs/latest/api/)
- [React](https://reactjs.org/docs/getting-started.html)
- [Docker Documentation](https://docs.docker.com/get-started/)
- [GitHub Action Documentations](https://docs.github.com/en/actions)
- Cloud Platform Documentation
    - [AWS](https://docs.aws.amazon.com/)
    - [Azure](https://learn.microsoft.com/en-us/azure/?product=popular)
    - [Google Cloud](https://cloud.google.com/docs)

The Project will challenge your skills in developing a full-stack application and automating its deployment, offering a comprehensive understanding of CI/CD practices in a comercial setting. 