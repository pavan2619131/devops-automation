pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                //checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/pavan2619131/devops-automation.git']]])
                //sh 'mvn clean install'
                //git branch: 'main', credentialsId: 'f6d59a9a-ba07-43d5-8e03-cc0a80b21ad4', url: 'https://github.com/pavan2619131/devops-automation.git'
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    //sh 'docker build -t suresh394/kubernetes .'
                     
                    sh 'docker build -t openjdk:8 .'
                
            }
        }
        // stage('Push image to hub'){
        //     steps{
        //         script{
        //             withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
        //             sh 'docker login -u suresh394 -p ${dockerhubpwd}'
                        
        //             }
        //             sh 'docker push suresh394/kubernetes'
        //         }
        //     }
        // }
        // stage('Deploy to K8s'){
        //     steps{
        //         script{
        //             kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
        //         }
        //     }
         }
    
    }    
}

