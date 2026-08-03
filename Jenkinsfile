pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/sindujaharikrishnan20-cyber/devops-project.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'cd app && docker build -t devops-project:v1 .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                docker rm -f devops-app || true
                docker run -d --name devops-app -p 8090:80 devops-project:v1
                '''
            }
        }
    }
}
