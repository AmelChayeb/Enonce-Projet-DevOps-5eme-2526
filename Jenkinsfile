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
        SONARQUBE_SERVER = 'SonarQubeServer' // The name you gave in Jenkins > Configure System
        SONAR_TOKEN = credentials('SONAR_TOKEN_ID') // Replace with your Sonar token ID in Jenkins
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

        stage('SonarQube Analysis') {
            environment {
                PATH = "${tool 'Maven3'}/bin:${env.PATH}"
            }
            steps {
                withSonarQubeEnv("${env.SONARQUBE_SERVER}") {
                    sh "mvn sonar:sonar -Dsonar.login=${env.SONAR_TOKEN}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${env.DOCKER_IMAGE} ."
                sh "docker push ${env.DOCKER_IMAGE}"
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
