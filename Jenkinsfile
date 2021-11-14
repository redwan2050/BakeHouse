pipeline {  
    agent any
    stages {
        stage('Build and push images ') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_id', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh "docker login -u '$USERNAME' -p '$PASSWORD'"
                sh "docker build -t kareemelkasaby/bakehouse:latest ."
                sh "docker push kareemelkasaby/bakehouse:latest" 
                }
            }
        }       
        stage('Deploy using k8s') {
            
            steps {
                sh "kubectl apply -f . --kubeconfig=/home/elkasaby/.kube/config"
            }
        }
    }
}