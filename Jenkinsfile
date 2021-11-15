pipeline{ 
    agent any
    tools{
        maven 'maven'
    }    
        
    stages{
        stage('build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'mygit', url: 'https://github.com/guiassamohammed/Devops-project-.git']]])
            }
        }
        stage('build a docker file'){
            steps{
                script{
                    
                    sh 'docker build -t 1993/mo60 .'
                }
                
            }
        }
        stage('Connect to my docker hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: '19931', variable: 'dockerhubpwd')]) {
                     sh  'docker login -u 19931 -p ${dockerhubpwd}'
                   }
                    sh 'docker push 19931/mo60'
                 
                }
                
            }
            
        }
        stage('deploy my docker container in k8s'){
            steps{
                
                sshagent(['K8s']) {
                sh "scp -o StrictHostKeyChecking=no k8s.yaml machine@k8sclusterip:<Put your directory"   
                script{
                    try{
                        sh "minikube start"
                        sh " ssh machine@k8sclusterip kubectl create -f ."
                    }catch(error){
                        sh " ssh machine@k8sclusterip  kubectl create -f ."
                        
                    }
                    
                }
                 }
                 
            }
            
            
        }
        
    }    
    
    
    
}
    
    
}
