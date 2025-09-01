pipeline {
    agent { label 'devOps' }

    environment {
        DOCKER_PASSWORD = credentials('DOCKER_PASSWORD')
        BACK_IMAGE_TAG = "mariemendoye/ml-backend:${BUILD_NUMBER}"
        BACK_IMAGE_LATEST = "mariemendoye/ml-backend:latest"
    }

    stages {

        stage('Connexion sur docker') {
            steps {
                script {
                    sh 'echo $DOCKER_PASSWORD | docker login -u mariemendoye --password-stdin'
                }
            }
        }

        stage('Build et push de l\'image Backend') {
            steps {
                script {
                        // Builder l'image avec le numéro du build 
                        sh "docker build -t ${BACK_IMAGE_TAG} ."

                        // Tag en latest
                        sh "docker tag ${BACK_IMAGE_TAG} ${BACK_IMAGE_LATEST}"

                        // Pusher les deux images
                        sh "docker push ${BACK_IMAGE_TAG}"
                        sh "docker push ${BACK_IMAGE_LATEST}"
                        }
                  }
        }

        stage('nettoyage') {
            steps {
                script {
                    // Supprission des images locales 
                    sh "docker rmi ${BACK_IMAGE_TAG} || true"
                    sh "docker rmi ${BACK_IMAGE_LATEST} || true"

                    // Suppression des images inutiles
                    sh "docker image prune -f"
                }
            }
        }
    }
}
