pipeline {
    agent { label 'iti-aswan' }

    stages {
        stage('build') {
            steps {
                echo 'build'
                withCredentials([usernamePassword(credentialsId: 'iti-aswan-dockerhub', usernameVariable: 'USERNAME_ITI', passwordVariable: 'PASSWORD_ITI')]) {
                    sh """
                        docker login -u ${USERNAME_ITI}  -p ${PASSWORD_ITI}
                        docker build -t ${USERNAME_ITI}/iti-aswan-bakehouse:${BUILD_NUMBER} .
                        docker push ${USERNAME_ITI}/iti-aswan-bakehouse:${BUILD_NUMBER}
                    """
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
                withCredentials([file(credentialsId: 'iti-aswan-kubeconfig', variable: 'KUBECONFIG')]) {
                    sh """
                        mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                        cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                        rm -f Deployment/deploy.yaml.tmp
                        kubectl apply -f Deployment --kubeconfig ${KUBECONFIG}
                    """
                }
            }
        }
    }
}
