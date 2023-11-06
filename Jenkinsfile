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
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Build'){
            steps {
                sh 'docker build -t epsdevops/reactapp:$BUILD_ID .'
            }
        }
        stage('Deliver'){
            steps {
                sh 'docker push epsdevops/reactapp:$BUILD_ID'
            }
        }
        stage('Cleanup'){
            steps {
                sh 'docker rmi epsdevops/reactapp:$BUILD_ID'
                sh 'docker logout'
            }
        }
    }
}
