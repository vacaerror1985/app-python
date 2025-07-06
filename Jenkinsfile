pipeline {
    agent any

    environment {
        DOCKER_IMAGE     = 'vacaerror/app-python'
        DOCKER_TAG       = 'v2.0'
        CONTAINER_NAME   = 'app-python'
        DOCKER_NETWORK   = 'app-python-network'
        HOST_PORT        = '5000'
        CONTAINER_PORT   = '5000'
    }

    stages {
        stage('Crear red Docker si no existe') {
            steps {
                echo '🔧 Verificando red Docker...'
                bat 'docker network inspect %DOCKER_NETWORK% >nul 2>&1 || docker network create %DOCKER_NETWORK%'
            }
        }

        stage('Detener y eliminar contenedor si existe') {
            steps {
                echo '🧹 Limpiando contenedor anterior si existe...'
                bat '''
                    docker container inspect %CONTAINER_NAME% >nul 2>&1 && (
                        docker container stop %CONTAINER_NAME%
                        docker container rm %CONTAINER_NAME%
                    ) || echo "No existe el contenedor '%CONTAINER_NAME%'."
                '''
            }
        }

        stage('Desplegar contenedor desde Docker Hub') {
            steps {
                echo "🐳 Desplegando imagen desde Docker Hub..."
                bat 'docker pull %DOCKER_IMAGE%:%DOCKER_TAG%'
                bat '''
                    docker run -d --name %CONTAINER_NAME% ^
                        --network %DOCKER_NETWORK% ^
                        -p %HOST_PORT%:%CONTAINER_PORT% ^
                        %DOCKER_IMAGE%:%DOCKER_TAG%
                '''
            }
        }
    }

    post {
        always {
            echo '✅ Pipeline completado.'
        }
    }
}
