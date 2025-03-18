# GitHub Actions: Implementing Continuous Integration

## Introduction
This project is part of the **GitHub Actions and CI/CD Course**, focusing on implementing **Continuous Integration (CI)** using GitHub Actions. The primary goal is to automate testing, ensure code quality, and validate changes efficiently before deployment.

## Why Continuous Integration?
Continuous Integration (CI) allows developers to frequently integrate code changes, run tests automatically, and identify issues early in the development cycle. By leveraging **GitHub Actions**, this project ensures a streamlined CI workflow.

## Prerequisites
Before proceeding, ensure you have the following:

- **Proficiency in YAML** (used for defining GitHub Actions workflows)
- **Experience with GitHub and GitHub Actions**
- **Knowledge of Node.js and npm**
- **Understanding of software testing concepts**
- **Familiarity with code quality tools** (e.g., ESLint)
- **Development Environment:**
  - Installed Git, Node.js, and a text editor (e.g., VS Code)
  - Internet access for cloning repositories

## Project Directory Structure
```
.
├── .github
│   ├── workflows
│   │   ├── deploy.yml
│   │   ├── ci.yml
│
├── api
│   ├── src
│   ├── .dockerignore
│   ├── Dockerfile
│   ├── app.test.js
│   ├── package.json
│
├── tests
│   ├── .dockerignore
│   ├── .env.example
│   ├── Dockerfile
│
├── webapp
│   ├── src
│   ├── .dockerignore
│   ├── Dockerfile
│   ├── .env.example
│   ├── .gitignore
│   ├── docker-compose.yml
│
├── README.md
```

## Necessary Files and Their Contents

### 1. **GitHub Actions CI Workflow (`.github/workflows/ci.yml`)**
```yaml
name: CI Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x, 14.x, 16.x]
    
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2
      
      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: ${{ matrix.node-version }}
      
      - name: Install Dependencies
        run: npm install
      
      - name: Run Tests
        run: npm test
      
      - name: Lint Code
        run: npx eslint .
```

### 2. **Dockerfile (`api/Dockerfile`)**
```dockerfile
FROM node:16-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

### 3. **Testing (`api/app.test.js`)**
```javascript
const request = require("supertest");
const app = require("./app");

test("GET / should return a welcome message", async () => {
  const res = await request(app).get("/");
  expect(res.statusCode).toBe(200);
  expect(res.text).toBe("Hello from Backend!");
});
```

### 4. **Environment Variables Example (`webapp/.env.example`)**
```
PORT=3000
DB_HOST=localhost
DB_USER=root
DB_PASS=secret
```

### 5. **ESLint Configuration (`.eslintrc.json`)**
```json
{
  "extends": "eslint:recommended",
  "env": {
    "node": true,
    "jest": true
  },
  "rules": {
    "no-unused-vars": "warn",
    "no-console": "off"
  }
}
```

### 6. **Docker Compose (`docker-compose.yml`)**
```yaml
version: "3.8"
services:
  api:
    build: ./api
    ports:
      - "3000:3000"
    env_file:
      - .env
```

## Running the Project
### 1. **Clone the Repository**
```sh
git clone https://github.com/Holuphilix/github-actions-ci-cd.git
cd github-actions-ci-cd
```

### 2. **Set Up Environment Variables**
```sh
cp .env.example .env
```

### 3. **Install Dependencies and Run Tests**
```sh
cd api
npm install
npm test
```

### 4. **Run the Application Locally**
```sh
npm start
```

### 5. **Run with Docker**
```sh
docker-compose up --build
```

## CI/CD Pipeline
The GitHub Actions workflow automatically runs:
1. **Builds the application** using different Node.js versions
2. **Installs dependencies**
3. **Runs tests**
4. **Checks code quality** using ESLint

## Conclusion
This project demonstrates the implementation of **Continuous Integration** using GitHub Actions. The automation ensures that every code update is validated, maintaining software quality and stability. By following this guide, developers can easily integrate similar workflows into their projects.

