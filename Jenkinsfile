pipeline {
    agent { label 'jenkins-ubuntu-slave' }
    stages {
        stage('build') {
            steps {
               withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                   sh """
                        docker login -u $USERNAME -p $PASSWORD
                        docker build -t kareemelkasaby/itimansbakehouse:${BUILD_NUMBER} .
                        docker push kareemelkasaby/itimansbakehouse:${BUILD_NUMBER}
                   """
               }
            }
        }
        stage('test') {
            steps {
                echo 'test'
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
            }
        }
    }
}
