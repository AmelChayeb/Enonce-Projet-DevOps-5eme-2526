pipeline {
    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    environment {
        GIT_REPO = 'https://github.com/AmelChayeb/Enonce-Projet-DevOps-5eme-2526.git'
        BRANCH = 'main'
    }

    stages {
        stage('Checkout') {
            steps {
                echo '📥 Clonage du dépôt Git...'
                git url: "${env.GIT_REPO}", branch: "${env.BRANCH}"
            }
        }

        stage('Clean') {
            steps {
                echo '🧹 Nettoyage du dossier target...'
                sh 'rm -rf target/* || true'
            }
        }

        stage('Build') {
            steps {
                echo '⚙️ Compilation du projet...'
                sh './mvnw clean compile'
            }
        }

        stage('Package') {
            steps {
                echo '📦 Création du livrable (.jar)...'
                sh './mvnw package'
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline terminé avec succès !'
        }
        failure {
            echo '❌ Erreur dans le pipeline.'
        }
    }
}
