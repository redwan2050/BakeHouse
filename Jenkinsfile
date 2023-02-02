pipeline {
    agent { label 'jenkins-ubuntu-slave' }
    stages {
        stage('build') {
            steps {
                echo 'build'
                sh "ls"
                sh "docker ps"
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
