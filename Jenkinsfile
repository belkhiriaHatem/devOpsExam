pipeline {
    agent any
    triggers { pollSCM('*/5 * * * *') }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dh_cred')
    }
    stages {
        stage('Checkout'){
            agent any
            steps{
                checkout scm
            }
        }
        stage('Init'){
            steps{
                bat 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Build'){
            steps {
                bat 'docker build -t epsdevops/reactapp:$BUILD_ID .'
            }
        }
        stage('Deliver'){
            steps {
                bat 'docker pubat epsdevops/reactapp:$BUILD_ID'
            }
        }
        stage('Cleanup'){
            steps {
                bat 'docker rmi epsdevops/reactapp:$BUILD_ID'
                bat 'docker logout'
            }
        }
    }
}
