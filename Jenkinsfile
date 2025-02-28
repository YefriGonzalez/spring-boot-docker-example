pipeline {
    agent any
    
    environment {
        IMAGE_NAME = "mi-backend"
        CONTAINER_NAME = "backend-container"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: env.BRANCH_NAME, credentialsId: 'github-credentials', url: 'https://github.com/YefriGonzalez/spring-boot-docker-example.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build & Run') {
            steps {
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
