# Github Action

> Auto-generated summaries from articles and videos. Last updated: 2026-08-18.

## Table of Contents

<!-- INDEX START -->
- [CI/CD with GitHub Actions: Deploying Node.js to a VPS with Docker](#cicd-with-github-actions-deploying-nodejs-to-a-vps-with-docker) — *2026-08-18 04:06*
<!-- INDEX END -->

---


## CI/CD with GitHub Actions: Deploying Node.js to a VPS with Docker

*Added: 2026-08-18 04:06*

**Source:** [https://www.youtube.com/watch?v=y7S2oSjJ8PA](https://www.youtube.com/watch?v=y7S2oSjJ8PA)

## Contents

- [Overview](#overview)
- [Understanding the Deployment Process](#understanding-the-deployment-process)
- [The Problem with Manual Deployment](#the-problem-with-manual-deployment)
- [Introduction to CI/CD and GitHub Actions](#introduction-to-cicd-and-github-actions)
- [Setting Up the Node.js Application](#setting-up-the-nodejs-application)
- [Dockerizing the Application](#dockerizing-the-application)
- [Setting Up Docker Compose](#setting-up-docker-compose)
- [GitHub Repository Setup](#github-repository-setup)
- [Provisioning a Private VPS](#provisioning-a-private-vps)
- [Installing Docker on the VPS](#installing-docker-on-the-vps)
- [Manual Deployment on the VPS](#manual-deployment-on-the-vps)
- [Automating Deployment with GitHub Actions](#automating-deployment-with-github-actions)
- [Generating and Configuring SSH Keys](#generating-and-configuring-ssh-keys)
- [Testing Automated CI/CD](#testing-automated-cicd)

## Overview

This guide demonstrates how to build a Continuous Integration/Continuous Deployment (CI/CD) pipeline using GitHub Actions to automatically deploy a Node.js application to a private Virtual Private Server (VPS) using Docker. The process involves setting up a Node.js application, containerizing it with Docker, configuring Docker Compose, provisioning a VPS, installing Docker on the VPS, and finally, writing a GitHub Actions workflow to automate the deployment on every code push.

## Understanding the Deployment Process

Traditionally, deploying an application involves several manual steps:

1.  **Code Development**: A developer writes code on their local machine.
2.  **Version Control**: The code is pushed to a remote version control system like GitHub.
3.  **Server Access**: The developer accesses the target server (e.g., a VPS) via SSH (Secure Shell) to establish a secure connection.
4.  **Code Retrieval**: The latest code is pulled from the GitHub repository onto the server.
5.  **Application Execution**: The application is then run on the server, often using tools like Docker Compose to manage containers.

## The Problem with Manual Deployment

The manual deployment process is repetitive and time-consuming. Every time a developer pushes new code to GitHub, they have to manually SSH into the server, pull the latest changes, and restart the application. This is inefficient and prone to human error.

## Introduction to CI/CD and GitHub Actions

**CI/CD (Continuous Integration/Continuous Deployment)** aims to automate the entire software delivery process.

**GitHub Actions** is a CI/CD platform built into GitHub. It allows you to automate workflows directly in your repository. You define these workflows using YAML files, which specify a series of steps (jobs) to be executed on certain events (like a code push).

In this context, GitHub Actions will:
1.  Detect a code push to the main branch.
2.  Run a predefined script on a GitHub-hosted runner (a virtual machine).
3.  The script will SSH into your private VPS.
4.  On the VPS, it will pull the latest code and restart the Dockerized application.

## Setting Up the Node.js Application

A simple Node.js Express application will be used for this demonstration.

1.  **Initialize Project**:
    ```bash
    npm init -y
    ```
2.  **Install Dependencies**:
    ```bash
    npm install express
    npm install @types/express --save-dev
    ```
3.  **Configure `package.json`**:
    Add a `start` script:
    ```json
    "scripts": {
      "start": "node index.js"
    },
    "type": "module"
    ```
    The `"type": "module"` allows using ES module syntax (import/export).
4.  **Create `index.js`**:
    This file contains a basic Express server.
    ```javascript
    import express from 'express';
    const app = express();
    const port = process.env.PORT || 8080;

    app.get('/', (req, res) => {
      res.json({ message: 'Hello from the server' });
    });

    app.listen(port, () => {
      console.log(`Server is up and running on port ${port}`);
    });
    ```

## Dockerizing the Application

To ensure consistent deployment across environments, the application is containerized using Docker.

1.  **Create `Dockerfile`**:
    This file contains instructions for building a Docker image.
    ```dockerfile
    FROM node:22-alpine
    WORKDIR /app
    COPY package*.json ./
    RUN npm install
    COPY . .
    EXPOSE 8080
    CMD ["node", "index.js"]
    ```
    *   `FROM node:22-alpine`: Uses a lightweight Node.js base image.
    *   `WORKDIR /app`: Sets the working directory inside the container.
    *   `COPY package*.json ./`: Copies package files to leverage Docker's layer caching for `npm install`.
    *   `RUN npm install`: Installs dependencies.
    *   `COPY . .`: Copies the rest of the application code.
    *   `EXPOSE 8080`: Informs Docker that the container listens on port 8080.
    *   `CMD ["node", "index.js"]`: Specifies the command to run the application when the container starts.
2.  **Test Docker Build and Run**:
    ```bash
    docker build -t api .
    docker run -p 8080:8080 api
    ```
    Verify by curling `http://localhost:8080`.

## Setting Up Docker Compose

Docker Compose is used to define and run multi-container Docker applications. For a single service, it simplifies management.

1.  **Create `docker-compose.yaml`**:
    ```yaml
    version: '3.8'
    services:
      app:
        build:
          context: .
          dockerfile: Dockerfile
        container_name: node-app
        restart: unless-stopped
        ports:
          - "8080:8080"
    ```
    *   `build`: Instructs Docker Compose to build the image from the current directory (`.`) using the `Dockerfile`.
    *   `container_name`: Assigns a specific name to the container.
    *   `restart: unless-stopped`: Ensures the container restarts automatically unless explicitly stopped.
    *   `ports`: Maps host port 8080 to container port 8080.
2.  **Test Docker Compose**:
    ```bash
    docker compose up -d
    ```
    Verify by curling `http://localhost:8080`. To stop, use `docker compose down`.

## GitHub Repository Setup

1.  **Create a New GitHub Repository**:
    Name it appropriately (e.g., `nodejs-app-deploy-gh-actions`). Set visibility to public or private as needed.
2.  **Initialize Git and Push Code**:
    *   Generate a `.gitignore` file for Node.js:
        ```bash
        npx gitignore node
        ```
    *   Initialize Git, add files, commit, and push to GitHub:
        ```bash
        git init
        git add .
        git commit -m "Initial commit"
        git remote add origin <your-repo-url>
        git push -u origin main
        ```

## Provisioning a Private VPS

A Virtual Private Server (VPS) provides a dedicated environment for hosting applications.

1.  **Choose a VPS Provider**: Hostinger KVM plans are recommended for their affordability and features.
2.  **Select a Plan and OS**: Choose a KVM plan (e.g., KVM 2), select a server location nearest to your users, and choose a plain Ubuntu OS (e.g., Ubuntu 22.04).
    *   *Note*: Use coupon codes `PIYUSH15` for 24-month plans (15% off) or `PIYUSH10` for 12-month plans (10% off).
3.  **Access the VPS**: Once provisioned, you'll get an IP address, username (usually `root`), and password. You can access it via SSH from your local terminal or Hostinger's web-based terminal.
4.  **Update System Packages**:
    ```bash
    sudo apt update
    sudo apt upgrade -y
    ```

## Installing Docker on the VPS

Docker needs to be installed on the VPS to run the containerized application.

1.  **Install Docker Engine on Ubuntu**:
    Follow the official Docker documentation or use these commands:
    ```bash
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg
    sudo install -m 0755 -d /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    echo \
      "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
      sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
    ```
2.  **Verify Docker Installation**:
    ```bash
    docker --version
    ```
    You should see Docker client and server versions.

## Manual Deployment on the VPS

Before automating, perform a manual deployment on the VPS to understand the steps GitHub Actions will replicate.

1.  **Clone the Repository**:
    ```bash
    git clone <your-repo-url>
    ```
2.  **Navigate to Application Directory**:
    ```bash
    cd nodejs-app-deploy-gh-actions # Or whatever your repo name is
    ```
3.  **Run Docker Compose**:
    ```bash
    docker compose up -d
    ```
4.  **Test Application**:
    Use `curl` from your local machine or the VPS to the server's IP address on port 8080:
    ```bash
    curl http://<your-vps-ip>:8080
    ```
5.  **Demonstrate Manual Update**:
    *   Change the message in `index.js` (e.g., "Hello from server V1").
    *   Push changes to GitHub: `git add .`, `git commit -m "V1 changes"`, `git push`.
    *   On the VPS, manually pull changes: `git pull`.
    *   Rebuild and restart Docker Compose: `docker compose up -d --build`.
    *   Verify with `curl`.

## Automating Deployment with GitHub Actions

Now, create the GitHub Actions workflow to automate the manual deployment steps.

1.  **Create Workflow File**:
    In your local repository, create the directory and file:
    `.github/workflows/deploy.yaml`
2.  **Define the Workflow**:
    ```yaml
    name: Deploy Node.js Application to Hostinger VPS

    on:
      push:
        branches:
          - main # Trigger on pushes to the 'main' branch

    jobs:
      deploy:
        runs-on: ubuntu-latest # The GitHub-hosted runner environment

        steps:
          - name: Checkout Code
            uses: actions/checkout@v4 # Action to clone the repository onto the runner

          - name: Deploy via SSH
            uses: appleboy/ssh-action@v1.0.0 # Action to SSH into the remote server
            with:
              host: ${{ secrets.SSH_HOST }} # VPS IP address from GitHub Secrets
              username: root # VPS username
              key: ${{ secrets.SSH_KEY }} # Private SSH key from GitHub Secrets
              script: | # Commands to execute on the VPS
                cd /root/nodejs-app-deploy-gh-actions # Navigate to your app directory
                git pull # Pull latest code
                docker compose up -d --build # Rebuild and restart Docker Compose
    ```
    *   `name`: A descriptive name for your workflow.
    *   `on: push: branches: [main]`: Specifies that the workflow runs on every push to the `main` branch.
    *   `jobs: deploy`: Defines a job named `deploy`.
    *   `runs-on: ubuntu-latest`: The job will run on a fresh Ubuntu virtual machine provided by GitHub.
    *   `steps`: A sequence of tasks to be executed.
        *   `Checkout Code`: Uses `actions/checkout@v4` to clone your repository onto the GitHub runner.
        *   `Deploy via SSH`: Uses `appleboy/ssh-action@v1.0.0` to establish an SSH connection to your VPS and execute commands.
            *   `host`, `username`, `key`: These are credentials for SSH access. It's crucial to store `host` (VPS IP) and `key` (private SSH key) as GitHub Secrets for security.
            *   `script`: Contains the commands to run on the VPS: `cd` into the app directory, `git pull` for latest code, and `docker compose up -d --build` to rebuild and restart the application.

## Generating and Configuring SSH Keys

For GitHub Actions to securely connect to your VPS, you need an SSH key pair.

1.  **Generate SSH Key Pair on Your Local Machine**:
    ```bash
    ssh-keygen -t rsa -b 4096 -C "github-action-key"
    ```
    *   When prompted for a file to save the key, you can specify a custom name (e.g., `~/.ssh/github_action_rsa`).
    *   Do **not** set a passphrase for the key if it's for automated use.
    *   This command will generate two files: `github_action_rsa` (private key) and `github_action_rsa.pub` (public key).
2.  **Copy Public Key to VPS**:
    *   Copy the content of `github_action_rsa.pub`.
    *   SSH into your VPS.
    *   Navigate to the `.ssh` directory: `cd ~/.ssh`. If it doesn't exist, create it: `mkdir ~/.ssh && chmod 700 ~/.ssh`.
    *   Append the public key to the `authorized_keys` file:
        ```bash
        echo "YOUR_PUBLIC_KEY_CONTENT" >> ~/.ssh/authorized_keys
        chmod 600 ~/.ssh/authorized_keys
        ```
        Replace `YOUR_PUBLIC_KEY_CONTENT` with the actual content of your `github_action_rsa.pub` file.
3.  **Add Private Key and VPS Host to GitHub Secrets**:
    *   Go to your GitHub repository -> `Settings` -> `Secrets and variables` -> `Actions`.
    *   Click `New repository secret`.
    *   Create a secret named `SSH_HOST` and paste your VPS IP address as its value.
    *   Create another secret named `SSH_KEY` and paste the **entire content** of your `github_action_rsa` (private key) file as its value.
    *   **Important**: Never commit your private or public SSH keys to your GitHub repository. Delete the generated key files from your local project directory after configuring them in GitHub Secrets.

## Testing Automated CI/CD

1.  **Commit and Push Workflow File**:
    ```bash
    git add .github/workflows/deploy.yaml
    git commit -m "Add GitHub Actions deploy workflow"
    git push origin main
    ```
    This push will trigger the GitHub Action.
2.  **Monitor GitHub Actions**:
    Go to your GitHub repository -> `Actions` tab. You should see your workflow running. Click on it to view the logs and ensure all steps (Checkout Code, Deploy via SSH) complete successfully.
3.  **Make a Code Change**:
    Modify `index.js` again (e.g., change message to "Hello from server V2 deployed").
4.  **Commit and Push Code Change**:
    ```bash
    git add index.js
    git commit -m "Update to V2"
    git push origin main
    ```
5.  **Verify Automated Deployment**:
    After the GitHub Action completes, use `curl` to check your VPS IP address:
    ```bash
    curl http://<your-vps-ip>:8080
    ```
    You should see the updated message ("Hello from server V2 deployed"), confirming the automated deployment.

---
