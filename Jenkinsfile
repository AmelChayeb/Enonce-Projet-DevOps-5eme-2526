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
        CONTAINER_NAME = 'mywebapp'
        SONARQUBE_SERVER = 'MySonarQube'
        SONAR_TOKEN = credentials('sonarqube-token')
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
        withSonarQubeEnv('MySonarQube') { // Name of SonarQube server in Jenkins
            // Use withCredentials to avoid insecure interpolation
            withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                withSonarQubeEnv('SonarQube') {
    sh 'mvn sonar:sonar -Dsonar.login=$SONAR_TOKEN'
}

            }
        }
    }
}


        stage('Quality Gate') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build Docker image
                    sh "docker build -t ${env.DOCKER_IMAGE} ."

                    // Push Docker image to Docker Hub
                    sh "docker push ${env.DOCKER_IMAGE}"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop and remove container if it exists
                    sh "if [ \$(docker ps -aq -f name=${env.CONTAINER_NAME}) ]; then docker rm -f ${env.CONTAINER_NAME}; fi"

                    // Run new container
                    sh "docker run -d -p 8082:80 --name ${env.CONTAINER_NAME} ${env.DOCKER_IMAGE}"
                }
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

