pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'vacaerror/api-monedastt'
        CONTAINER_NAME = 'api-monedastt'
        DOCKER_NETWORK = 'apimonedas_red'
        HOST_PORT = '9090'
        CONTAINER_PORT = '8080'
    }

    stages {
     stage('Construir imagen Docker') {
            steps {
                echo '🐳 Construyendo imagen Docker en raíz del proyecto...'
                bat "docker build -t ${IMAGE_NAME}:${DOCKER_TAG} -f Dockerfile ."
            }
        }

        stage('Limpiar contenedor existente') {
            steps {
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

        stage('Desplegar contenedor') {
            steps {
                bat "docker run --network ${DOCKER_NETWORK} --name ${CONTAINER_NAME} -p ${HOST_PORT}:${CONTAINER_PORT} -d ${DOCKER_IMAGE}"
            }
        }
    }
}