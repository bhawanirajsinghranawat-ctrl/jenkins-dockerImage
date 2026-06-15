pipeline {
    agent any

    environment {
        IMAGE_NAME = "bhawani608/jenkins1:latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url:  'https://github.com/bhawanirajsinghranawat-ctrl/jenkins-dockerImage.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    bat "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'credentials-dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        bat "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    }
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    bat "docker push ${IMAGE_NAME}"
                }
            }
        }

        stage('Cleanup') {
            steps {
                script {
                    bat "docker rmi ${IMAGE_NAME}"
                }
            }
        }
    }
}
