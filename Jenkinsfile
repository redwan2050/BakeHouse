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
                configFileProvider([configFile(fileId: 'kubeconfig', variable: 'k8s_config')]) {
                    sh """
                        kubectl delete -f .  --kubeconfig=$k8s_config
                        kubectl apply -f .  --kubeconfig=$k8s_config
                    """
                }
            }
        }
    }
}
