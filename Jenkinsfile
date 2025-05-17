pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds-id') // Jenkins > Credentials'dan alınmalı
        DOCKER_IMAGE = 'seymahandekli/devops-hw4:latest'
    }

    triggers {
        githubPush() // triggerd push and merge
    }

    stages {
        stage('Clone repository') {
            steps {
                git url: 'https://github.com/seymahandekli/devops-hw4.git', branch: 'master'
            }
        }

        stage('Build JAR with Gradle') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('Build Docker image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push Docker image') {
            steps {
                sh 'docker push $DOCKER_IMAGE'
            }
        }

        stage('Apply Kubernetes Deployment') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }

        stage('Apply Kubernetes Service') {
            steps {
                sh 'kubectl apply -f service.yaml'
            }
        }
    }
}
