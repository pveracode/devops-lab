pipeline {

    agent any

    stages {

        stage('Build Application') {
            steps {
                sh './mvnw clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devops-lab:latest .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop devops-lab-app || true
                docker rm devops-lab-app || true

                docker run -d \
                --name devops-lab-app \
                -p 8080:8080 \
                devops-lab:latest
                '''
            }
        }
    }
}