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

        stage('Image Building') {                   
            steps {
                sh 'docker build -t devops2022.azurecr.io/ullis-image:$GIT_COMMIT .' //image wird auf Jenkins VM gebaut aus Dockerfile (im git Repo) und getagt
                sh 'docker push devops2022.azurecr.io/ullis-image:$GIT_COMMIT'              //image wird auf ACR gepushed
                sh 'docker rmi devops2022.azurecr.io/ullis-image:$GIT_COMMIT'               // image wird von Jenkins VM gelöscht
            }
        }

        stage('deploy to k8s') {
            agent {
                docker {
                    image 'alpine/k8s:1.23.16'                                  // in diesem Image laufen die Befehle an K8s, da hier kubectl verstanden werden kann
                }
            }

            environment{
                KUBECONFIG = credentials('k8s_config')                          // credentials von unserem k8s Cluster, in Jenkins angelegt
            }

            steps {
                sh 'echo $KUBECONFIG'
                sh 'kubectl --kubeconfig=$KUBECONFIG apply -f namespace.yaml'
                sh 'sed "s|REPLACE_ME|devops2022.azurecr.io/ullis-image:$GIT_COMMIT|" -i nginx-deployment.yaml'
                sh 'kubectl --kubeconfig=$KUBECONFIG apply -f nginx-deployment.yaml'            // das zieht das Image aus ACR, drinnen benannt, und baut pods draus.
               // sh 'kubectl --kubeconfig=$KUBECONFIG set image -n ullisjenkinsspace deployment/ullis-jenkins-deployment jenkins=devops2022.azurecr.io/ullis-image:$GIT_COMMIT'                
                sh 'kubectl --kubeconfig=$KUBECONFIG apply -f service.yaml'
                sh 'kubectl --kubeconfig=$KUBECONFIG apply -f loadbalancer.yaml'

               


            }
        }

    }    
    
}
