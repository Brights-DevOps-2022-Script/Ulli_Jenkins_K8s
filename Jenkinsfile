pipeline {
    agent {
        docker {
            image 'devops2022.azurecr.io/alpine-test:latest'
        }
    }
    environment {
        ACRCreds = credentials('acr_creds')
        KUBECONFIG = credentials('k8s_config')
    }
    
    stages {
        
        stage('ACR Login') {
            steps{
                sh 'docker login devops2022.azurecr.io -u $ACRCreds_USR -p $ACRCreds_PSW'
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
    
}
