pipeline {

    agent any

    environment {
        IMAGE_NAME = "devops-project"
        IMAGE_TAG  = "v1"
        CONTAINER_NAME = "devops-app"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Code is already checked out by Jenkins SCM"
            }
        }

        stage('Docker Build') {
            steps {
                echo "Building Docker Image"

                sh '''
                cd app
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }

        stage('Docker Test') {
            steps {
                echo "Running Docker Container"

                sh '''
                docker rm -f ${CONTAINER_NAME} || true

                docker run -d \
                --name ${CONTAINER_NAME} \
                -p 8090:80 \
                ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }

        stage('Docker Verify') {
            steps {
                echo "Checking Container Status"

                sh '''
                docker ps
                docker images
                '''
            }
        }

    }

    post {

        success {
            echo "Build Successful - Application Deployed"
        }

        failure {
            echo "Build Failed - Check Logs"
        }

    }
}
