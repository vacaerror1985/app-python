pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'vacaerror/app-python'
        DOCKER_TAG = 'latest'
        CONTAINER_NAME = 'app-python'
        DOCKER_NETWORK = 'app-python-network'
        HOST_PORT = '5000'
        CONTAINER_PORT = '5000'
    }

    stages {
        stage('Limpiar contenedor existente') {
            steps {
                echo '🧹 Verificando contenedor anterior...'
                script {
                    catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                        bat """
                        docker container inspect ${CONTAINER_NAME} >nul 2>&1 && (
                            docker container stop ${CONTAINER_NAME}
                            docker container rm ${CONTAINER_NAME}
                        ) || echo "No existe el contenedor '${CONTAINER_NAME}'."
                        """
                    }
                }
            }
        }

        stage('Desplegar desde Docker Hub') {
            steps {
                echo "🐳 Descargando y ejecutando contenedor desde Docker Hub..."
                bat "docker pull ${DOCKER_IMAGE}:${DOCKER_TAG}"
                bat "docker run --network ${DOCKER_NETWORK} --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} -d ${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }
    }

    post {
        always {
            echo '✅ Pipeline finalizado.'
        }
    }
}
