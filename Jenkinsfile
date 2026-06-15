pipeline {
    agent any

    environment {
        IMAGE_NAME = "bhawani608/jenkins1:latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/bhawanirajsinghranawat-ctrl/jenkins-dockerImage.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'credentials-dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    bat '''
                    docker logout
                    docker login -u "%DOCKER_USER%" -p "%DOCKER_PASS%"
                    '''
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                bat 'docker push %IMAGE_NAME%'
            }
        }

        stage('Cleanup') {
            steps {
                bat 'docker rmi %IMAGE_NAME%'
            }
        }
    }

    post {
        success {
            echo 'Docker image pushed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check console output.'
        }
    }
}
