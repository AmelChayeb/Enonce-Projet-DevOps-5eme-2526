pipeline {
    agent any


    tools {
        jdk 'JDK21'         // or whatever name you gave in Jenkins
        maven 'Maven3'      // must match the name above
    }

   
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AmelChayeb/Enonce-Projet-DevOps-5eme-2526.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project...'
                sh ' mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build("amelchayeb/mywebapp:latest")
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests (mock step for now)...'
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        dockerImage.push()
                    }
                }
            }
        }
    }
}
