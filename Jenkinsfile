pipeline {
  agent { label 'jenkins-ubuntu-slave' }
  parameters {
        choice(name: 'ENV', choices: ['dev', 'test', 'prod',"release"])
  }
  stages {
    stage('build') {
      when {
        expression { return params.ENV == "release"}
      }
      steps {
        script {
          if (params.ENV == "release") {
            withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
              sh """
                  docker login -u ${USERNAME} -p ${PASSWORD}
                  docker build -t kareemelkasaby/vfbakehouse:${BUILD_NUMBER} .
                  docker push kareemelkasaby/vfbakehouse:${BUILD_NUMBER}
                  echo ${BUILD_NUMBER} > ../bakehouse-build-number.txt
              """
            }
          } else {
            sh 'echo "user choosed ${ENV}"'
          }
        }
      }
    }
    stage('deploy') {
      when {
        expression { return params.ENV == "dev" || params.ENV == "test" || params.ENV == "prod"  }
      }
      steps {
        script {
          if (params.ENV == "dev" || params.ENV == "test" || params.ENV == "prod") {
            withCredentials([file(credentialsId: 'kubernetes_kubeconfig', variable: 'KUBECONFIG')]) {
              sh """
                  export BUILD_NUMBER=\$(cat ../bakehouse-build-number.txt)
                  mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                  cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                  rm -f Deployment/deploy.yaml.tmp
                  kubectl apply -f Deployment --kubeconfig=${KUBECONFIG}
                """
            }
          }
          def STR = "iti,3month,nodejs,slave,abdelrahman"
          def ARR = STR.split(",")
          for(i in ARR){
            echo i
          }
        }
      }
    }
  }
}
