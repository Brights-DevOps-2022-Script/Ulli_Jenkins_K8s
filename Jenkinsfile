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

        stage('make k8s run') {
            steps {
                sh 'kubectl apply -f namespace.yaml'
                sh "kubectl apply -f nginx-deployment.yaml"
                sh "kubectl apply -f service.yaml"
                sh "kubectl apply -f loadbalancer.yaml"
            }
        }
    }
        
        stage('push docker imgae zu acr'){
            steps {
                sh 'echo hier wird zu acr gepushed'
            }
        }
    
}
