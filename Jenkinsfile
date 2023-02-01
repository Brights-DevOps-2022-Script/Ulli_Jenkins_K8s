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
        stage('build docker image from Dockerfile'){
            steps {
                sh 'echo hier wird docker image gebaut'
                sh 'docker build -t devops2022.azurecr.io/nginx:ullis-image .'
            }
        }
        
        stage('push docker imgae zu acr'){
            steps {
                sh 'echo hier wird zu acr gepushed'
            }
        }
    }
}
