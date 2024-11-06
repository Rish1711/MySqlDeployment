# MySQL CI/CD Pipeline with Jenkins and Docker

This repository provides a Jenkins pipeline that builds and deploys a MySQL Docker container securely. It avoids exposing credentials in the `Dockerfile` by using Jenkins’ credential management.

## Repository Structure

- **Dockerfile**: Defines the Docker image for MySQL (without credentials).
- **Jenkinsfile**: Pipeline script to build the Docker image, create, and run the MySQL container securely.
- **README.md**: Documentation on the project setup and usage.

## Requirements

- **Jenkins** with Docker installed on the agent.
- **Jenkins Credentials** configured for secure environment variable management.

## Setup Guide

1. **Clone the Repository**

   Clone this repository to your Jenkins server or local machine if you are testing locally.

   ```sh
   git clone https://github.com/your-username/my-mysql-ci-cd.git
