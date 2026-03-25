pipeline {
    agent any

    environment {
        IMAGE_1 = 'nginx:latest'
        IMAGE_2 = 'mysql:8.0'
        IMAGE_3 = 'redis:alpine'
        IMAGE_4 = 'node:20'
        IMAGE_5 = 'python:3.12'
    }

    stages {

        stage('Pull nginx:latest') {
            steps {
                echo "⬇️  Pull en cours : nginx:latest"
                sh "docker pull ${IMAGE_1}"
                echo "✅ nginx:latest téléchargée avec succès"
            }
        }

        stage('Pull mysql:8.0') {
            steps {
                echo "⬇️  Pull en cours : mysql:8.0"
                sh "docker pull ${IMAGE_2}"
                echo "✅ mysql:8.0 téléchargée avec succès"
            }
        }

        stage('Pull redis:alpine') {
            steps {
                echo "⬇️  Pull en cours : redis:alpine"
                sh "docker pull ${IMAGE_3}"
                echo "✅ redis:alpine téléchargée avec succès"
            }
        }

        stage('Pull node:20') {
            steps {
                echo "⬇️  Pull en cours : node:20"
                sh "docker pull ${IMAGE_4}"
                echo "✅ node:20 téléchargée avec succès"
            }
        }

        stage('Pull python:3.12') {
            steps {
                echo "⬇️  Pull en cours : python:3.12"
                sh "docker pull ${IMAGE_5}"
                echo "✅ python:3.12 téléchargée avec succès"
            }
        }

        stage('Vérification des images') {
            steps {
                echo "📋 Liste des images téléchargées :"
                sh "docker images | grep -E 'nginx|mysql|redis|node|python'"
            }
        }
    }

    post {
        success {
            echo '🎉 Succès ! Les 5 images sont bien téléchargées sur Jenkins.'
        }
        failure {
            echo '❌ Erreur ! Consulte Console Output pour voir le problème.'
        }
    }
}
