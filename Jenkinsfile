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
        stage('deploy') {
            steps {
                withCredentials([file(credentialsId: 'kubernetes_kubeconfig', variable: 'KUBECONFIG')]) {
              sh """
                  mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                  cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                  rm -f Deployment/deploy.yaml.tmp
                  kubectl apply -f Deployment --kubeconfig=${KUBECONFIG}
                """
            }
            }
        }
    }
}
