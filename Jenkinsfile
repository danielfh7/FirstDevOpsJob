pipeline {
    agent any
     environment {
        AWS_ACCESS_KEY_ID     = credentials('jenkis_aws_key_id')
        AWS_SECRET_ACCESS_KEY = credentials('jenkis_aws_key_secret')
    } 
 stages {
     
     stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/danielfh7/FirstDevOpsJob'
                
            }
        }
     
     stage ('Build') {
         steps {
             bat(script: 'mvn -f pom.xml clean install', returnStdout: true);
         }
        }
        
     stage ('Docker Build') {
           steps{
             // docker login  
             bat(script: 'docker build -t danielfh7/servicenet6v1 .' , returnStdout:true);
             bat(script: 'docker push danielfh7/servicenet6v1' , returnStdout:true);  
           }
        }
        
         stage ('K8S Deploy') {
            steps {
              bat(script: 'aws configure set region us-east-1',returnStdout: true);
              bat(script: 'kubectl config use-context arn:aws:eks:us-east-1:931651913632:cluster/aforo255-cluster-eks --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
              bat(script: 'Kubectl delete --all pods --kubeconfig=%KUBE_PATH_CONFIG% & kubectl apply -f Devops.yaml --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
            }
               
            
        }
    
      
     }
}