# Java Maven CI/CD Pipeline with Trivy & SonarQube Quality Gate

![Build Status](https://github.com/bhuwan1233/ci-cd-pipeline-code-quality/actions/workflows/ci.yml/badge.svg)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=bhuwan1233_ci-cd-pipeline-code-quality&metric=alert_status)](https://sonarcloud.io/summary/overall?id=bhuwan1233_ci-cd-pipeline-code-quality)

An automated Continuous Integration (CI) pipeline built with **GitHub Actions**, **Trivy**, and **SonarCloud** for a Java Maven application. This pipeline automates building, unit testing, security vulnerability scanning, static code analysis, and quality gate enforcement on every commit.

---

## 🛠️ Tech Stack & Tools

* **Language:** Java 17 (JDK 17)
* **Build Tool:** Apache Maven 3.x
* **CI/CD Orchestration:** GitHub Actions
* **Security Scanner:** Trivy (Filesystem vulnerability scanning)
* **Code Quality & Static Analysis:** SonarCloud / SonarQube
* **Secrets Management:** GitHub Repository Secrets

---

## 🔄 Pipeline Workflow Architecture

```text
[ Developer Push / Pull Request ]
               │
               ▼
   1. Checkout Source Code
               │
               ▼
   2. Setup JDK 17 (Temurin)
               │
               ▼
   3. Compile Project (mvn compile)
               │
               ▼
   4. Run Unit Tests (mvn test)
               │
               ▼
   5. Package Application (.jar)
               │
               ▼
   6. Security Scan (Trivy FS Scan)
               │
               ▼
   7. Code Quality Analysis (SonarCloud Scan)
               │
               ▼
   8. Quality Gate Verification




Step 1 — Local Project Setup
Created project directory and checked Java 17 and Maven environments:

Bash
java -version
mvn -version

Generated Maven project structure:

Bash
mvn archetype:generate "-DgroupId=com.example" "-DartifactId=ci-cd-project" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DarchetypeVersion=1.5" "-DinteractiveMode=false"
cd ci-cd-project
Verified application build locally:

Bash
mvn clean test
mvn package
Step 2 — Source Control Management
Initialized Git and connected to the remote repository:

Bash
git init
git add .
git commit -m "Initial Java Maven project setup"
git branch -M main
git remote add origin [https://github.com/bhuwan1233/ci-cd-pipeline-code-quality.git](https://github.com/bhuwan1233/ci-cd-pipeline-code-quality.git)
git push -u origin main
Step 3 — GitHub Actions & Integrations Configuration
SonarCloud Setup:

Imported repository into SonarCloud.

Generated Personal Access Token under SonarCloud account settings.

Created GitHub Repository Secret named SONAR_TOKEN.

Pipeline Definition (.github/workflows/ci.yml):

YAML
name: CI CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    name: Build, Scan and Quality Gate
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Setup JDK 17
        uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: '17'
          cache: maven

      - name: Compile project
        run: mvn -B compile

      - name: Run tests
        run: mvn -B test

      - name: Package application
        run: mvn -B package

      - name: Run Trivy Vulnerability Scanner (FS)
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          ignore-unfixed: true
          format: 'table'
          severity: 'CRITICAL,HIGH'

      - name: SonarQube / SonarCloud Scan
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        run: mvn -B verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=bhuwan1233_ci-cd-pipeline-code-quality -Dsonar.organization=bhuwan1233 -Dsonar.host.url=[https://sonarcloud.io](https://sonarcloud.io)

      - name: SonarQube Quality Gate Check
        uses: sonarsource/sonarqube-quality-gate-action@master
        timeout-minutes: 5
        with:
          scanMetadataReportFile: target/sonar/report-task.txt
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
🖼️ Pipeline Execution Proof & Screenshots
GitHub Actions Successful Workflow Execution
<img width="1271" height="666" alt="Screenshot 2026-09-29 135440" src="https://github.com/user-attachments/assets/c841e182-6f0e-4165-92a9-eba7e062108e" />



SonarCloud Analysis & Quality Gate Result
<img width="1366" height="669" alt="Screenshot 2026-09-29 135547" src="https://github.com/user-attachments/assets/0eb10c0c-7a61-4704-8a20-2e84e3f4a175" />



🚀 How to Run Locally
Prerequisites
JDK 17

Apache Maven 3.x

Git

Local Build Commands
Bash
# Clone the repository
git clone [https://github.com/bhuwan1233/ci-cd-pipeline-code-quality.git](https://github.com/bhuwan1233/ci-cd-pipeline-code-quality.git)
cd ci-cd-pipeline-code-quality

# Execute local compile and test suite
mvn clean test

# Create executable JAR package
mvn package

---
