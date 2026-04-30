pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "manojpatil1831/flask-app"
        TAG = "latest"
    }
    stages {
        stage('Checkout Code') {
            agent any  // Uses default agent (has Git)
            steps {
                git branch: 'main', url: 'https://github.com/manojpatil9019888189-glitch/Finacplus.git'
            }
        }
        stage('Build Docker Image') {
            agent {
                docker {
                    image 'docker:24.0'  // Official Docker image with CLI
                    args '-v /var/run/docker.sock:/var/run/docker.sock'  // Access host Docker
                }
            }
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$TAG .'
            }
        }
        // Similar for Push stage...
    }
}
