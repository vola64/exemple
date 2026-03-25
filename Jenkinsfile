pipeline {
    agent any

    environment {
        DOCKERHUB_USER        = 'vola64'
        DOCKERHUB_CREDS       = 'dockerhub-credentials'

        TARGET_REGISTRY       = 'registry.example.com'     // ← change ici
        TARGET_REGISTRY_CREDS = 'target-registry-credentials'
        TARGET_NAMESPACE      = 'exemple'

        // ← mets ici tes 5 vraies images Docker Hub
        IMAGE_1 = 'vola64/image-one:latest'
        IMAGE_2 = 'vola64/image-two:latest'
        IMAGE_3 = 'vola64/image-three:latest'
        IMAGE_4 = 'vola64/image-four:latest'
        IMAGE_5 = 'vola64/image-five:latest'
    }

    stages {

        stage('Login Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${DOCKERHUB_CREDS}",
                    usernameVariable: 'DH_USER',
                    passwordVariable: 'DH_PASS'
                )]) {
                    sh 'echo "$DH_PASS" | docker login -u "$DH_USER" --password-stdin'
                }
                echo '✅ Connecté à Docker Hub'
            }
        }

        stage('Pull images depuis Docker Hub') {
            steps {
                script {
                    def images = [
                        env.IMAGE_1,
                        env.IMAGE_2,
                        env.IMAGE_3,
                        env.IMAGE_4,
                        env.IMAGE_5
                    ]
                    images.each { image ->
                        echo "⬇️  Pull : ${image}"
                        sh "docker pull ${image}"
                    }
                }
            }
        }

        stage('Login Registry cible') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "${TARGET_REGISTRY_CREDS}",
                    usernameVariable: 'TR_USER',
                    passwordVariable: 'TR_PASS'
                )]) {
                    sh 'echo "$TR_PASS" | docker login ${TARGET_REGISTRY} -u "$TR_USER" --password-stdin'
                }
                echo '✅ Connecté au registry cible'
            }
        }

        stage('Tag & Push vers registry cible') {
            steps {
                script {
                    def images = [
                        env.IMAGE_1,
                        env.IMAGE_2,
                        env.IMAGE_3,
                        env.IMAGE_4,
                        env.IMAGE_5
                    ]
                    images.each { image ->
                        def targetImage = "${TARGET_REGISTRY}/${TARGET_NAMESPACE}/${image}"
                        sh "docker tag ${image} ${targetImage}"
                        sh "docker push ${targetImage}"
                        echo "✅ Image migrée : ${targetImage}"
                    }
                }
            }
        }

        stage('Nettoyage local') {
            steps {
                script {
                    def images = [
                        env.IMAGE_1,
                        env.IMAGE_2,
                        env.IMAGE_3,
                        env.IMAGE_4,
                        env.IMAGE_5
                    ]
                    images.each { image ->
                        def targetImage = "${TARGET_REGISTRY}/${TARGET_NAMESPACE}/${image}"
                        sh "docker rmi ${image} ${targetImage} || true"
                    }
                }
                echo '🧹 Images locales supprimées'
            }
        }
    }

    post {
        success { echo '🎉 Toutes les images ont été migrées avec succès !' }
        failure { echo '❌ Une erreur est survenue. Vérifie les logs.' }
        always  {
            sh 'docker logout || true'
            sh "docker logout ${TARGET_REGISTRY} || true"
        }
    }
}
