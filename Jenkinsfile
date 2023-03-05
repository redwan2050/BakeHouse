pipeline {
    agent { label 'iti-aswan' }

    stages {
        stage('build') {
            steps {
                echo 'build'
                withCredentials([usernamePassword(credentialsId: 'iti-aswan-dockerhub', usernameVariable: 'USERNAME-ITI', passwordVariable: 'PASSWORD-ITI')]) {
                    sh """
                        docker login -u ${USERNAME-ITI}  -p ${PASSWORD-ITI}
                        docker build -t ${USERNAME-ITI}/iti-aswan-bakehouse:${BUILD_NUMBER}
                        docker push ${USERNAME-ITI}/iti-aswan-bakehouse:${BUILD_NUMBER}
                    """
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
            }
        }
    }
}
