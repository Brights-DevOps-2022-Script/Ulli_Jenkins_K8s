pipeline {
    agent any
    environment {
        ACRCreds = credentials('acr_creds')
       
    }
    
    stages {
        
        stage('ACR Login') {
            steps{
                sh 'docker login devops2022.azurecr.io -u $ACRCreds_USR -p $ACRCreds_PSW'
            }
        }

        stage('deploy') {
            agent {
                docker {
                    image 'alpine/k8s:1.23.16'
                }
            }

            environment{
                KUBECONFIG = credentials('k8s_config')
            }

            steps {
                sh 'echo $KUBECONFIG'
                sh 'kubectl --kubeconfig=$KUBECONFIG apply -f namespace.yaml'
                sh "kubectl --kubeconfig=$KUBECONFIG apply -f nginx-deployment.yaml"
                sh "kubectl --kubeconfig=$KUBECONFIG apply -f service.yaml"
                sh "kubectl --kubeconfig=$KUBECONFIG apply -f loadbalancer.yaml"
            }
        }

    }    
    
}
