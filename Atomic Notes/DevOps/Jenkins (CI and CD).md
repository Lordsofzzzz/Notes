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
Open-source automation server for building, testing, and deploying software.

### Pipeline Models
- **Declarative Pipeline**: A structured and simple model for building CI/CD workflows using a `pipeline` block.
- **Scripted Pipeline**: A powerful and flexible model for complex pipeline logic using Groovy script.

### Declarative Pipeline Script Example
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

### Integration and SCM
- **GitHub Webhook**: Automatically triggers the pipeline on every commit.
- **Maven & Selenium**: Jenkins uses Maven to build artifacts and Selenium to run automated UI tests on the built code.
- **SCM Integration**: Jenkins fetches source code from SCM (e.g., Git) for every build.

## Associative Trails
Jenkins provides the "glue" for automating the entire software lifecycle. This note captures the core pipeline models and their practical application in automating builds, tests, and deployments, which refines the manual workflows described in [[Git (Version Control)]].

## Connections
- [[DevOps Engineering (MOC)]]
- [[Git (Version Control)]]
- [[Docker and Containerization]]
- [[Kubernetes (K8s) Architecture]]

## Sources
- [[DevOps Engineering Exam Preparation.md]]
