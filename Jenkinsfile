pipeline {

    agent any

    options {
        skipDefaultCheckout(true)
        timeout(time: 15, unit: 'MINUTES')
    }

    environment {

        AWS_REGION = "us-east-1"

        ECR_REPO = "123456789012.dkr.ecr.us-east-1.amazonaws.com/devops-project"

        IMAGE_NAME = "devops-project"

        IMAGE_TAG = "latest"

        CONTAINER_NAME = "devops-app"
    }


    stages {


        stage('Checkout Code') {

            steps {

                git branch: 'main',
                    url: 'https://github.com/sindujaharikrishnan20-cyber/devops-project.git'

            }
        }


        stage('Docker Build') {

            steps {

                sh '''
                cd app

                docker build \
                -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''

            }
        }


        stage('ECR Login') {

            steps {

                sh '''

                aws ecr get-login-password \
                --region ${AWS_REGION} | \
                docker login \
                --username AWS \
                --password-stdin ${ECR_REPO}

                '''

            }
        }



        stage('Docker Tag') {

            steps {

                sh '''

                docker tag \
                ${IMAGE_NAME}:${IMAGE_TAG} \
                ${ECR_REPO}:${IMAGE_TAG}

                '''

            }
        }



        stage('Push Image To ECR') {

            steps {

                sh '''

                docker push \
                ${ECR_REPO}:${IMAGE_TAG}

                '''

            }
        }



        stage('Deploy Container Test') {

            steps {

                sh '''

                docker rm -f ${CONTAINER_NAME} || true


                docker run -d \
                --name ${CONTAINER_NAME} \
                -p 8090:80 \
                ${IMAGE_NAME}:${IMAGE_TAG}

                '''

            }
        }



        stage('Verify') {

            steps {

                sh '''

                docker images

                docker ps

                '''

            }
        }

    }



    post {

        success {

            echo "CI/CD Pipeline Completed Successfully"

        }


        failure {

            echo "Pipeline Failed - Check Console Output"

        }

    }

}
