pipeline {
    agent {
        docker {
            image 'devops2022.azurecr.io/alpine-simon:latest'
        }
    }
    environment {
        ACRCreds = credentials('acr_creds')
        KUBECONFIG = credentials('k8s_config')
    }
    
    stages {
        
        stage('ACR Login') {
            steps{
                sh 'docker login devops2022.azurecr.io -u $ACR_CRED_USR -p $ACR_CRED_PSW'
            }
        }

        stage('make k8s run') {
            steps {
                sh 'kubectl apply -f namespace.yaml'
                sh "kubectl apply -f nginx-deployment.yaml"
                sh "kubectl apply -f service.yaml"
                sh "kubectl apply -f loadbalancer.yaml"
            }
        }

    
    
}
