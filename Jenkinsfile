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

    stage('SonarQube Analysis') {
        steps {
            withSonarQubeEnv('sonarqube') {
                sh '''
                ./mvnw sonar:sonar \
                  -Dsonar.projectKey=devops-lab \
                  -Dsonar.projectName=devops-lab
                '''
            }
        }
    }

    stage('Build Docker Image') {
        steps {
            sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
        }
    }

    stage('Push Docker Image') {
        steps {
            sh 'docker push ${IMAGE_NAME}:${IMAGE_TAG}'
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