pipeline {

    agent any

    environment {
        IMAGE_NAME = "localhost:5000/devops-lab"
        IMAGE_TAG = "latest"
        CONTAINER_NAME = "devops-lab-app"
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code'
            }
        }

        stage('Build Application') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh '''
                docker push ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true

                docker run -d \
                --name ${CONTAINER_NAME} \
                -p 8080:8080 \
                ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }
    }

    post {

        success {
            echo 'Deployment completed successfully'
        }

        failure {
            echo 'Pipeline failed'
        }
    }
}