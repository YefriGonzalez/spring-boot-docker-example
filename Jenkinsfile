pipeline {
    agent any
    
    environment {
        IMAGE_NAME = "mi-backend"
        CONTAINER_NAME = "backend-container"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Rama actual: ${env.GIT_BRANCH.replaceAll(/^origin\//, '')}"
                // Hacemos checkout de la rama usando GIT_BRANCH
                git branch: "${env.GIT_BRANCH.replaceAll(/^origin\//, '')}", credentialsId: 'github-credentials', url: 'https://github.com/YefriGonzalez/spring-boot-docker-example.git'
            }
        }

        stage('Build') {
            steps {
                withMaven() {
                    sh "mvn clean package -DskipTests"
                }
            }
        }

        stage('Docker Build & Run') {
            steps {
                // Comandos para construir y ejecutar el contenedor Docker
                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                docker build -t $IMAGE_NAME .
                docker run -d -p 8080:8080 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo 'Backend desplegado correctamente en local'
        }
        failure {
            echo 'Error en el pipeline'
        }
    }
}
