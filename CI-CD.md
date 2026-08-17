# Ci Cd

> Auto-generated summaries from articles and videos. Last updated: 2026-08-17.

## Table of Contents

<!-- INDEX START -->
- [Understanding CI/CD: Continuous Integration and Continuous Delivery](#understanding-cicd-continuous-integration-and-continuous-delivery) — *2026-08-17 22:52*
<!-- INDEX END -->

---


## Understanding CI/CD: Continuous Integration and Continuous Delivery

*Added: 2026-08-17 22:52*

**Source:** [https://www.youtube.com/watch?v=gLptmcuCx6Q](https://www.youtube.com/watch?v=gLptmcuCx6Q)

## Overview
CI/CD, which stands for Continuous Integration and Continuous Delivery/Deployment, is a core concept in DevOps. It automates the various stages of the software development lifecycle, from code changes to deployment, making the process faster, more reliable, and less error-prone. This automation helps development teams deliver new features and bug fixes to users more frequently and efficiently.

## What is CI/CD?

CI/CD is an automation of the software development process, triggered by every code change. It consists of two main parts:

### Continuous Integration (CI)
Continuous Integration automates the **build** and **test** stages of the software development lifecycle.
- **Process**: When a developer commits and pushes code changes to their branch, the CI pipeline automatically builds the application and runs various tests (unit tests, integration tests, regression tests).
- **Purpose**: To quickly detect and address integration issues and bugs. If tests fail, the developer is immediately notified to fix the code before it's merged into the main branch.
- **Benefits**: Reduces "integration hell" by ensuring code is always in a working state, improves code quality, and speeds up the development cycle.

### Continuous Delivery (CD)
Continuous Delivery automates the **release** and **deployment** stages. It ensures that code changes are always ready for release to users.
- **Process**: After successful CI, the application is automatically prepared for deployment. This typically involves deploying to a staging environment.
- **Key Characteristic**: Continuous Delivery includes a **manual approval step** before deploying to a production environment. A team lead or manager must approve the changes on staging before they are automatically deployed to production.
- **Purpose**: To ensure that the application is consistently deliverable to users, with a safety net of manual review for critical deployments.

### Continuous Deployment (CD)
Continuous Deployment is an extension of Continuous Delivery.
- **Key Characteristic**: Unlike Continuous Delivery, Continuous Deployment **eliminates the manual approval step**. After successful testing and deployment to staging, the application is automatically deployed to production without human intervention.
- **Use Case**: Often used by organizations with well-established DevOps teams and less sensitive data (e.g., streaming services like Netflix). For highly sensitive applications (e.g., banking), Continuous Delivery with manual approval is generally preferred.

## The Software Development Lifecycle (SDLC) and CI/CD
The SDLC involves several steps that CI/CD automates:

1.  **Develop**: Developers write code for new features or bug fixes.
2.  **Commit & Push**: Developers commit their changes to their own branches in a version control system (like Git) and push them.
3.  **Build**: The application code, along with its dependencies and libraries, is compiled into deployable artifacts (e.g., binary files, executable files, `.apk` for Android).
4.  **Test**: Various tests are performed:
    *   **Unit Tests**: Performed by developers on individual code units.
    *   **Integration Tests**: Check if different components work correctly together.
    *   **Regression Tests**: Ensure new changes don't break existing functionality.
    *   **End-to-End Tests**: Performed after deployment to an environment (e.g., staging) and include security tests, smoke tests, alpha/beta tests.
5.  **Release**: Making the application available to end-users.
6.  **Deploy**: Making the application available on a specific environment. Common environments include:
    *   **Development (Dev)**: For developers only.
    *   **Staging**: A pre-production environment for demos and final testing by managers/CTOs.
    *   **Production (Prod)**: For all end-users.

## Why Do We Need CI/CD? (Problems with Manual Processes)

Before CI/CD, the entire SDLC was largely manual, leading to significant problems:

*   **Integration Hell**: Multiple developers working on the same application would commit changes to their branches. Manually merging these changes often led to conflicts and errors, consuming a lot of time and introducing bugs.
*   **Error-Prone and Time-Consuming Testing**: Manual testing is slow and prone to human error. It's difficult to re-perform hundreds of tests manually for every small change, leading to bugs reaching users.
*   **Infrequent Releases**: Manual testing and deployment processes cause code freezes and delays, preventing frequent releases of new features to users.
*   **Difficult Rollbacks**: If a bug was introduced in production during a manual deployment, rolling back to a previous stable version was also a manual, time-consuming process, leading to application downtime or users encountering bugs.
*   **Limited Deployment Times**: Due to the risks and manual effort, deployments were often avoided on Fridays or weekends, limiting flexibility.

CI/CD addresses these issues by automating the process, bringing reliability and speed to the entire software development lifecycle.

## How CI/CD Solves These Problems

*   **Continuous Integration**: Automates build and test processes for every code commit. This means developers get immediate feedback on their changes, catching bugs early and ensuring that code integrated into the main branch is always functional.
*   **Continuous Delivery/Deployment**: Automates the release and deployment process, enabling frequent and reliable delivery of new features to users. This allows for multiple deployments per day if needed.

## Implementing CI/CD Pipelines

CI/CD pipelines are implemented by writing automation scripts, often in YAML format. These scripts define the sequence of commands and instructions to be executed at each stage (build, test, deploy).

**Example (GitHub Actions):**
A YAML script can define a workflow that builds and tests Node.js code. When a new change is pushed, this workflow starts, running the defined commands. If all tests pass, a green tick indicates success. If tests fail, the exact step and issue are highlighted, allowing developers to quickly fix the problem.

## CI/CD Tools

Several tools are available to set up CI/CD pipelines:

*   **GitHub Actions**:
    *   Released in 2019, native to GitHub.
    *   Very popular and intuitive, especially if the code base is already on GitHub.
    *   Recommended for first-time CI/CD setup due to its ease of use and integration with Git.
*   **Jenkins**:
    *   Released in 2011, an older and widely used tool.
    *   A job orchestrator with extensive customization options.
    *   Popular for companies that started CI/CD before GitHub Actions.
    *   Can be harder to maintain compared to newer tools.
*   **Other Tools**: CircleCI, Travis CI, GitLab CI/CD, Bamboo, etc.

## Deployment Strategies with CI/CD

Even with extensive testing, bugs can reach production. CI/CD can be combined with different deployment strategies to minimize downtime and user impact:

1.  **Blue/Green Deployment**:
    *   Maintains two identical, separate environments: "Blue" (current version) and "Green" (new version).
    *   All user traffic is initially directed to the Blue environment.
    *   The new version is deployed to the Green environment.
    *   Once the Green environment is ready, all traffic is switched from Blue to Green.
    *   If issues arise with the new version, traffic can be instantly reverted to the stable Blue environment.
    *   **Benefit**: Near-zero downtime and easy rollback.

2.  **Canary Deployment**:
    *   Gradually rolls out the new version to a small subset of users (e.g., 5-10%).
    *   Monitors the new version's performance and stability with this small group.
    *   If stable, the rollout is progressively expanded to more users.
    *   If issues occur, only a small percentage of users are affected, and the rollout can be halted or rolled back.
    *   **Benefit**: Reduces the blast radius of potential issues.

3.  **Rolling Deployment**:
    *   Deploys the new version incrementally to a subset of server instances within a single environment.
    *   A load balancer distributes requests across multiple instances.
    *   The new version is deployed to a few instances while others still run the old version.
    *   If the new version is stable on those instances, it's gradually deployed to more instances until all are updated.
    *   **Benefit**: Allows for gradual updates with minimal impact, as old versions remain available on other instances during the rollout.

## Conclusion

CI/CD, especially when integrated with tools like Docker and Kubernetes, simplifies the development and deployment process for scalable applications and large teams. It enhances reliability, speeds up delivery, and improves overall software quality by automating critical stages of the SDLC.

---
