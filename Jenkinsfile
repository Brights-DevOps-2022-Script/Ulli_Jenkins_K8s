pipeline {
    agent {
        docker {
            image 'alpine/k8s:1.23.16'
        }
    }
    environment {
        ACRCreds = credentials('acr_creds')
        KUBECONFIG = credentials('k8s_config')
    }
    stages {

        stage('Deploy Nginx') {
            steps {
                sh 'kubectl apply -f namespace.yaml'
                // sh "kubectl apply -f nginx-deployment.yaml"
                // sh "kubectl apply -f nginx-service.yaml"
                // sh "kubectl get pod -n felixheureka"
            }
        }
    }
}
