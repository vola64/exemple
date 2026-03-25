pipeline {
    agent any

    stages {

        stage('Pull nginx:latest') {
            steps {
                sh 'docker pull nginx:latest'
                echo '✅ nginx:latest téléchargée'
            }
        }

        stage('Pull mysql:8.0') {
            steps {
                sh 'docker pull mysql:8.0'
                echo '✅ mysql:8.0 téléchargée'
            }
        }

        stage('Pull redis:alpine') {
            steps {
                sh 'docker pull redis:alpine'
                echo '✅ redis:alpine téléchargée'
            }
        }

        stage('Pull node:20') {
            steps {
                sh 'docker pull node:20'
                echo '✅ node:20 téléchargée'
            }
        }

        stage('Pull python:3.12') {
            steps {
                sh 'docker pull python:3.12'
                echo '✅ python:3.12 téléchargée'
            }
        }

        stage('Vérification') {
            steps {
                sh "docker images | grep -E 'nginx|mysql|redis|node|python'"
            }
        }
    }

    post {
        success {
            echo '🎉 Les 5 images sont téléchargées avec succès !'
        }
        failure {
            echo '❌ Erreur ! Consulte Console Output.'
        }
    }
}

