pipeline {
    agent any

    environment {
        IMAGE_NAME = "murasame11/filebrowser"
        VERSION = "1.0.0"
        FULL_TAG = "${IMAGE_NAME}:${VERSION}"
        DOCKERHUB_CREDENTIALS_ID = "dockerhub_id"
    }

    stages {
        stage('Crear la imagen') {
            steps {
                script {
                    echo "Creando la imagen ${FULL_TAG}"
                    sh "docker build -t ${FULL_TAG} ."
                }
            }
        }

        stage('Remover contenedores previos') {
            steps {
                script {
                    sh """
                    docker rm -f filebrowser || true
                    """
                }
            }
        }

        stage('Iniciar Contenedor') {
            steps {
                script {
                    sh """
                    docker run -d --name filebrowser -p 80:80 ${FULL_TAG}
                    """
                }
            }
        }

        stage('Test Container') {
            steps {
                script {
                    echo "Waiting for container to start..."
                    sleep 5
                    sh '''
                    if curl --fail http://localhost:80; then
                        echo "FileBrowser is running"
                    else
                        echo "FileBrowser not responding" && exit 1
                    fi
                    '''
                }
            }
        }

        stage('Push a DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDENTIALS_ID,
                                                 usernameVariable: 'DOCKER_USER',
                                                 passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push ${FULL_TAG}
                    docker logout
                    """
                }
            }
        }
    }
}
