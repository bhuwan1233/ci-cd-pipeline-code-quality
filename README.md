# Java Maven CI/CD Pipeline with Trivy & SonarQube Quality Gate

![Build Status](https://github.com/bhuwan1233/ci-cd-pipeline-code-quality/actions/workflows/ci.yml/badge.svg)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=bhuwan1233_ci-cd-pipeline-code-quality&metric=alert_status)](https://sonarcloud.io/summary/overall?id=bhuwan1233_ci-cd-pipeline-code-quality)

An end-to-end Continuous Integration pipeline built using **GitHub Actions**, **Trivy**, and **SonarCloud** to automate building, security scanning, code quality analysis, and quality gate verification for a Java Maven application.

---

## 🛠️ Tech Stack & Tools

* **Language:** Java 17
* **Build Tool:** Apache Maven
* **CI/CD Orchestration:** GitHub Actions
* **Security Scanner:** Trivy (Filesystem vulnerability scanning)
* **Code Quality & Static Analysis:** SonarCloud / SonarQube
* **Secrets Management:** GitHub Repository Secrets

---

## 🔄 CI/CD Pipeline Architecture

```text
[ Developer Push / PR ]
          │
          ▼
   1. Checkout Code
          │
          ▼
   2. Setup JDK 17
          │
          ▼
   3. Compile & Run Tests (Maven)
          │
          ▼
   4. Package Application (.jar)
          │
          ▼
   5. Security Scan (Trivy FS)
          │
          ▼
   6. Code Quality Analysis (SonarCloud)
          │
          ▼
   7. Quality Gate Enforcement
