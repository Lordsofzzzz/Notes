---
status: seedling
tags: [devops, automation, ci-cd]
created: 2026-04-10
updated: 2026-04-10
---

# Jenkins (CI and CD)

## Summary
An open-source automation server used to implement continuous integration and continuous delivery (CI/CD) pipelines.

## Details
Jenkins is the primary automation engine for modern CI/CD pipelines, integrating development workflows with automated testing and deployment.

### CI/CD Pipeline Stages
- **Source**: SCM (e.g., Git) commit triggers the pipeline via webhooks.
- **Build**: Compiling source code and creating build artifacts (e.g., `.jar`, `.war`).
- **Test**: Automated validation through unit, integration, and UI tests.
- **Deploy**: Pushing validated artifacts to a staging or production environment.

### Pipeline Models
- **Declarative Pipeline**: A structured model using a `pipeline` block, emphasizing "what" to do rather than "how."
- **Scripted Pipeline**: A flexible model using Groovy script for complex logic (e.g., loops, dynamic stages).

### Declarative Pipeline Example
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps { sh 'mvn clean package' }
        }
        stage('Test') {
            steps { sh 'mvn test' }
        }
        stage('Deploy') {
            steps { echo 'Deploying application...' }
        }
    }
}
```

### Integration & Automated Testing
- **Maven Integration**: Jenkins triggers Maven to build Java applications and run unit tests.
- **Selenium Integration**: After a successful build, Jenkins runs Selenium scripts (often in a headless browser) to validate user flows (login, navigation).
- **GitHub Webhook**: Automatically triggers a "build on push" to ensure every commit is validated.

### Job Configuration (Freestyle)
1. **SCM**: Configure the Git repository URL.
2. **Build Trigger**: Set to "GitHub hook trigger for GITScm polling."
3. **Build Step**: Invoke Maven targets (`clean package`).
4. **Post-build**: Archive artifacts or send notifications.

## Associative Trails
Jenkins provides the "glue" for automating the entire software lifecycle. This note captures the core pipeline models and their practical application in automating builds, tests, and deployments, which refines the manual workflows described in [[Git (Version Control)]].

## Connections
- [[DevOps Engineering (MOC)]]
- [[Git (Version Control)]]
- [[Docker and Containerization]]
- [[Kubernetes (K8s) Architecture]]

## Sources
- DevOps Engineering Exam Preparation.md
