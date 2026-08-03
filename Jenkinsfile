pipeline {

    agent any


    options {
        skipDefaultCheckout(true)
        timeout(time: 20, unit: 'MINUTES')
    }


    environment {

        AWS_REGION = "us-east-1"

        ECR_REPO = "940348258780.dkr.ecr.us-east-1.amazonaws.com/devops-project"

        IMAGE_NAME = "devops-project"

        IMAGE_TAG = "latest"

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



        stage('Deploy To Kubernetes') {

            steps {

                sh '''

                kubectl apply -f kubernetes/deployment.yaml

                kubectl apply -f kubernetes/service.yaml


                kubectl rollout restart deployment devops-app


                kubectl get pods

                kubectl get svc

                '''

            }
        }



        stage('Verify Deployment') {

            steps {

                sh '''

                kubectl rollout status deployment/devops-app

                '''

            }
        }

    }



    post {


        success {

            echo "================================="
            echo " CI/CD Deployment Successful "
            echo "================================="

        }


        failure {

            echo "Deployment Failed - Check Logs"

        }

    }

}
