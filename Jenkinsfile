pipeline {
    agent any

    environment {
        IMAGE_NAME = 'murasame11/filebrowser'
        IMAGE_TAG = '1.0.0'
        CONTAINER_NAME = 'filebrowser'
        DOCKERHUB_CREDENTIALS_ID = 'dockerhub_id'
    }

    stages {

        stage('Crear la imagen') {
            steps {
                script {
                    echo "Creando la imagen ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Remover contenedores previos') {
            steps {
                script {
                    sh "docker rm -f ${CONTAINER_NAME} || true"
                }
            }
        }

        stage('Iniciar Contenedor') {
            steps {
                script {
                    sh "docker run -d --name ${CONTAINER_NAME} -p 80:80 ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Test Container') {
            steps {
                script {
                    echo "Waiting for container to start..."
                    sleep 5
                    sh '''
                    CONTAINER_IP=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' filebrowser)
                    echo "Probing container at $CONTAINER_IP:80..."
                    if curl --fail http://$CONTAINER_IP:80; then
                        echo "✅ FileBrowser está corriendo correctamente."
                    else
                        echo "❌ FileBrowser no responde." && exit 1
                    fi
                    '''
                }
            }
        }

        stage('Push a DockerHub') {
            when {
                expression {
                    return currentBuild.currentResult == 'SUCCESS'
                }
            }
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                        sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}


