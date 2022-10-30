pipeline {
  agent { label 'jenkins-ubuntu-slave' }
  parameters {
        choice(name: 'ENV', choices: ['dev', 'test', 'prod',"release"])
  }
  stages {
    stage('build') {
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
          }
        }
      }
    }
    stage('deploy') {
      steps {
        script {
          sh """
              export BUILD_NUMBER=\$(cat ../bakehouse-build-number.txt)
              mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
              cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
              rm -f Deployment/deploy.yaml.tmp
              kubectl apply -f Deployment
            """
        }
      }
    }
  }
}
