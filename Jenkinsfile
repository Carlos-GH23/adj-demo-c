pipeline {
    agent any

    
    stages {
        //Parar todos los servicios
        stage('Parando todos los servicios'){
            steps {
                bat '''
                    docker compose -p adj-demo-c down || true
                '''
            }
        
    }

// Eliminar las imágenes creadas por ese proyecto
        stage('Eliminando imágenes anteriores...') {
            steps {
                bat '''
                    for /f "tokens=*" %%i in ('docker images --filter "label=com.docker.compose.project=adj-demo-c" -q') do (
                        docker rmi -f %%i
                    )
                    if errorlevel 1 (
                        echo No hay imagenes por eliminar
                    ) else (
                        echo Imagenes eliminadas correctamente
                    )
                '''
            }
        }
        //Bajar la actualizacion
        stage('Actualizando...'){
            steps{
                checkout scm
            }
        }


        //Levantar y desplegar el proyecto
        stage('Construyendo y desplegando...'){
            steps {
                bat '''
                    docker compose up --build -d
                '''
            }

        }
    }


    post {
        success {
            echo 'Pipeline ejecutado exitosamente.'
        }

        failure {
            echo 'Error al ejecutar el pipeline.'
        }

        always {
            echo 'Pipeline finalizado.'
        }
    }
}