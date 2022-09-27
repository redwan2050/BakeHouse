pipeline {
  agent { label 'jenkins-ubuntu-slave' }
  stages {
    stage('build') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
            sh """
                docker login -u ${USERNAME} -p ${PASSWORD}
                docker build -t kareemelkasaby/vfbakehouse:${BUILD_NUMBER} .
                docker push kareemelkasaby/vfbakehouse:${BUILD_NUMBER}
            """
          }
        }
      }
      stage('deploy') {
        steps {
          script {
            withCredentials([file(credentialsId: "kubernetes_kubeconfig", variable: 'KUBECONFIG')]) {
              sh """
                  envsubst < Deployment/deploy.yaml | tee Deployment/deploy.yaml 
                  kubectl apply -f Deployment --kubeconfig=${KUBECONFIG}
              """
            }
          }
        }
      }
    }
  }
}
