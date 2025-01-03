pipeline {
    agent any

    environment {
        // Set environment variables
        DOCKER_IMAGE = 'myapp:latest'
        K8S_NAMESPACE = 'default'
        SLACK_CHANNEL = '#deployments'
    }

    stages {
        // Stage 1: Pull the code from Git
        stage('Checkout Code') {
            steps {
                checkout scmGit(
                branches: [[name: 'master']],
                userRemoteConfigs: [[url: 'https://github.com/jenkinsci/git-plugin.git']])            }
        }

        // Stage 2: Run tests
        stage('Run Tests') {
            steps {
                script {
                    // Install dependencies and run tests (e.g., with Jest)
                    sh 'npm install'
                    sh 'npm test'
                }
            }
        }

        // Stage 3: Build Docker Image
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        // Stage 4: Push Docker Image (optional)
        stage('Push Docker Image') {
            steps {
                script {
                    // Push to Docker Hub or any other Docker registry
                    sh 'docker push ${DOCKER_IMAGE}'
                }
            }
        }

        // Stage 5: Deploy to Minikube
        stage('Deploy to Minikube') {
            steps {
                script {
                    // Set kubectl context for Minikube
                    sh 'kubectl config use-context minikube'
                    // Apply Kubernetes deployment YAML
                    sh 'kubectl apply -f k8s/deployment.yaml'
                }
            }
        }
    }

    post {
        success {
            // Send a Slack notification on success
            slackSend(channel: SLACK_CHANNEL, message: 'Deployment successful!')
        }
        failure {
            // Send a Slack notification on failure
            slackSend(channel: SLACK_CHANNEL, message: 'Deployment failed!')
        }
    }
}
