pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'Maven3'
    }

    environment {
        GIT_REPO = 'https://github.com/AmelChayeb/Enonce-Projet-DevOps-5eme-2526.git'
        BRANCH = 'main'
        DOCKER_IMAGE = 'amelchayeb/mywebapp:1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: "${env.GIT_REPO}", branch: "${env.BRANCH}"
            }
        }

        stage('Build Maven Project') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${env.DOCKER_IMAGE} ."
            }
        }

        stage('Run Docker Container') {
            steps {
                sh "docker stop mywebapp || true"
                sh "docker rm mywebapp || true"
                sh "docker run -d -p 8082:80 --name mywebapp ${env.DOCKER_IMAGE}"
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline finished successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
