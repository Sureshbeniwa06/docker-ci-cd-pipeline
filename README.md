# docker-ci-cd-pipeline

# A CI/CD pipeline project for automating Docker image builds and deployments using Jenkins and GitHub.

## Here's the workflow diagram for your CI/CD pipeline project. Let me know if you need any modifications or further enhancements!

![workflow diagram](https://github.com/user-attachments/assets/509d679a-6055-4052-9f1b-e4848efea5b9)



# CI/CD Pipeline for Docker Image Deployment

This project demonstrates a CI/CD pipeline that automates the process of pulling code from a GitHub repository, building a Docker image, and pushing it to Docker Hub. The pipeline is implemented using Jenkins.

## Features

- Automated code cloning from GitHub.
- Docker image build automation.
- Seamless Docker Hub integration for image deployment.
- Configurable pipeline for continuous integration and delivery.

## Prerequisites

Before starting, ensure you have the following installed and configured:

1. **Git**
2. **Docker**
3. **Jenkins** with the following plugins:
   - Git Plugin
   - Docker Pipeline Plugin
4. **Docker Hub** account

## Setup Instructions

### Step 1: Clone the Repository

Clone this repository to your local machine:

```bash
git clone https://github.com/Sureshbeniwa06/docker-ci-cd-pipeline.git
```

### Step 2: Configure Jenkins

1. **Install Jenkins**: Follow [this guide](https://www.jenkins.io/doc/book/installing/) to install Jenkins on your server.
2. **Add Docker Credentials**:
   - Go to `Manage Jenkins > Manage Credentials > Global Credentials`.
   - Add your Docker Hub username and password.
3. **Create a New Pipeline Job**:
   - Navigate to `New Item > Pipeline`.
   - Paste the following pipeline script:

```groovy
pipeline {
    agent any
    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub'  // Replace with your Docker Hub credentials ID
        DOCKER_IMAGE = 'suresh202/e-website:latest'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Sureshbeniwa06/Docker-images-on-localhost-running-how-.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }
    }
}
```

### Step 3: Test the Pipeline

1. Trigger the pipeline by clicking **Build Now** in Jenkins.
2. Monitor the progress in the **Console Output**.

### Step 4: Automate Triggering (Optional)

Set up a webhook in your GitHub repository to trigger the pipeline automatically on code pushes:

1. Go to your GitHub repository settings.
2. Navigate to **Webhooks** and click **Add webhook**.
3. Enter the Jenkins webhook URL:
   ```
   http://10.0.2.15:8080//github-webhook/
   ```
4. Select the **Just the push event** option and save.


## Future Enhancements

- Add automated testing before building the Docker image.
- Support for multi-environment deployments.
- Integration with Kubernetes for container orchestration.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.








