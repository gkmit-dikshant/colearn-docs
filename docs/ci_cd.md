# CI/CD Pipelines

Continuous Integration and Continuous Deployment (CI/CD) help the CoLearn project ship features faster, safer, and more reliably. This page explains all CI/CD workflows configured in GitHub Actions, including testing, SAST scans, and automated deployment to the staging environment.

---

## Continuous Integration (CI)

CI ensures that every change pushed to the repository is tested, scanned, and validated before reaching production.

### 1. GitHub Repository
All source code is maintained in a GitHub repository. Developers create feature branches and push changes through Pull Requests (PRs).

### 2. Automated Testing
Whenever a PR is created or code is pushed to `dev`, `staging`, or `main`, the test suite runs automatically.

The CI tests include:

- Unit Tests  
- Jest-based test suites  

This ensures new changes do not break existing functionality.

### 3. Static Application Security Testing (SAST)
PRs to `staging` and `dev` must pass the **Trivy SAST scan**, which checks for:

- Secrets committed to the repository  
- Misconfigured files  
- Critical and High severity vulnerabilities  

If vulnerabilities are detected, the PR cannot be merged.

### 4. Code Review
Every Pull Request requires review and approval from mentor.

Review checks include:

- Code quality  
- Maintainability  
- Security concerns  
- Consistency with project standards  

### 5. Build Verification
After all tests and SAST checks pass, the code is considered valid and ready for deployment.

---

## Continuous Deployment (CD)

CD automates deployment to the staging environment using GitHub Actions, SSH, and PM2 on AWS EC2.

### 1. Automated Staging Deployment
Whenever code is pushed to the `staging` branch:

1. Trivy SAST scan executes.  
2. If successful, the deployment pipeline connects to the EC2 instance using SSH.  
3. The server pulls the latest staging code, installs dependencies, and restarts PM2.

This ensures consistent and reliable deployments.

### 2. Production Deployment (Manual Approval)
Production deployments follow a controlled process:

- Code merges into the `main` branch.
- A human reviews all updates.
- Deployment to production is triggered manually.

This avoids accidental or unverified production releases.

### 3. Monitoring and Rollbacks
After each deployment:

- PM2 monitors logs and application health.

---

# GitHub Actions Pipelines

Below are the exact CI/CD pipelines used in the CoLearn project.

---

## 1. Staging Deployment Pipeline

```yaml
name: Deploy to EC2 (Staging)

on:
  push:
    branches:
      - staging

jobs:
  trivy-sast:
    name: Trivy SAST Scan Before Deploy
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run Trivy SAST
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: "fs"
          path: "."
          scanners: "secret,config"
          exit-code: 1
          severity: "CRITICAL,HIGH"

  deploy:
    name: Deploy to EC2
    runs-on: ubuntu-latest
    needs: trivy-sast

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup SSH
        uses: webfactory/ssh-agent@v0.9.0
        with:
          ssh-private-key: ${{ secrets.EC2_SSH_KEY }}

      - name: Deploy to EC2
        run: |
          ssh -o StrictHostKeyChecking=no ${{ secrets.EC2_USER }}@${{ secrets.EC2_HOST }} << 'EOF'

            echo "---- Pulling latest changes ----"
            cd /server/colearn-backend-api
            git fetch --all
            git reset --hard origin/staging

            echo "---- Installing dependencies ----"
            npm install --production

            echo "---- Restarting PM2 ----"
            pm2 restart all

            echo "---- Deployment Completed Successfully ----"
          EOF
```

## 2. Test Pipeline
```yml
name: Run Tests on Pull Request

on:
  pull_request:
    branches:
      - staging
      - main
      - dev
  push:
    branches:
      - staging
      - dev
      - main

jobs:
  test:
    name: Run Unit & Integration Tests
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Use Node.js
        uses: actions/setup-node@v4
        with:
          node-version: "18"

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Upload test results
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: test-results
          path: test-results/

```

## 3. Trivy SAST Scan Pipeline

```yaml
name: SAST Scan

on:
  pull_request:
    branches:
      - staging
      - dev

jobs:
  trivy-sast:
    name: Trivy SAST Scan
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run Trivy SAST
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: "fs"
          path: "."
          scanners: "secret,config"
          exit-code: 1
          severity: "CRITICAL,HIGH"

```