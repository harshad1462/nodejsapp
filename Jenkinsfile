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


    }


}
