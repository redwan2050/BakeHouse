pipeline {
    agent any

    stages {
        stage('test') {
            steps {
                echo 'test'
                sh "ls"
                sh "docker ps"
            }
        }
        stage('build') {
            steps {
                echo 'build'
                sh """
                    ls
                    echo ${build_number}
                """
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
            }
        }
    }
}
