pipeline {
    agent any

    environment {
        DOCKER_IMAGE     = 'vacaerror/app-python'
        DOCKER_TAG       = 'latest'
        CONTAINER_NAME   = 'app-python'
        DOCKER_NETWORK   = 'app-python-network'
        HOST_PORT        = '5000'
        CONTAINER_PORT   = '5000'
    }

   
    stages {
        stage(' Build') {
            steps {
                echo '\u001B[34mCompilando la aplicación...\u001B[0m'
            }
        }

        stage(' Browser Tests') {
            parallel {
                stage('Chrome') {
                    steps {
                        echo 'Test en Chrome completado'
                    }
                }
                stage('Firefox') {
                    steps {
                        echo 'Test en Firefox completado'
                    }
                }
                stage('Internet Explorer') {
                    steps {
                        echo 'Test en Internet Explorer completado'
                    }
                }
                stage('Safari') {
                    steps {
                        echo 'Test en Safari completado'
                    }
                }
            }
        }

        stage(' Static Analysis') {
            steps {
                echo ' Ejecutando análisis estático del código (simulado)'
            }
        }

        stage('Docker Deploy') {
            steps {
                script {
                    echo '🔧 Creando red si no existe...'
                    bat 'docker network inspect %DOCKER_NETWORK% >nul 2>&1 || docker network create %DOCKER_NETWORK%'

                    echo 'Limpiando contenedor anterior...'
                    bat '''
                        docker container inspect %CONTAINER_NAME% >nul 2>&1 && (
                            docker container stop %CONTAINER_NAME%
                            docker container rm %CONTAINER_NAME%
                        ) || echo "Contenedor no existente"
                    '''

                    echo '⬇Descargando imagen desde Docker Hub...'
                    bat 'docker pull %DOCKER_IMAGE%:%DOCKER_TAG%'

                    echo ' Ejecutando contenedor...'
                    bat '''
                        docker run -d --name %CONTAINER_NAME% ^
                            --network %DOCKER_NETWORK% ^
                            -p %HOST_PORT%:%CONTAINER_PORT% ^
                            %DOCKER_IMAGE%:%DOCKER_TAG%
                    '''
                }
            }
        }
    }

    post {
        always {
            echo ' Pipeline finalizado.'
        }
    }
}
