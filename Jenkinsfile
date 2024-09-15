pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    environment {
        ECR_REPO_URI = '533267326662.dkr.ecr.us-east-1.amazonaws.com/new-one-testing-rep'  // Replace with your ECR repository URI
        AWS_REGION = 'us-east-1'      // Replace with your AWS region
        IMAGE_NAME = "${JOB_NAME}-image"    // Docker image name
        IMAGE_TAG = 'latest'                // Docker image tag
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
        
        stage('Build Docker Image') {
            steps {
                script {
                    // // Construct Docker image name based on job name
                    // def imageName = "${JOB_NAME}-image"
                    // echo "Building Docker image: ${imageName}"
                    
                    // // Build Docker image from the Dockerfile in the root of the workspace
                    // docker.build(imageName)

                     // Construct Docker image name based on job name
                    def imageName = "${JOB_NAME}-image"
                    echo "Building Docker image with BuildKit: ${imageName}"
                    
                    // Enable BuildKit and build Docker image
                    sh "DOCKER_BUILDKIT=1 docker build -t ${imageName} ."
                }
            }
        }
         stage('Verify Docker Image') {
            steps {
                script {
                    // List Docker images to verify the build
                    sh 'docker images'
                }
            }
        }

          stage('Login to ECR') {
            steps {
                script {
                    // Login to Amazon ECR

                   // aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 533267326662.dkr.ecr.us-east-1.amazonaws.com
                     sh '''
                    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 533267326662.dkr.ecr.us-east-1.amazonaws.com
                    // $(aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${ECR_REPO_URI})
                     '''
                }
            }
        }
        
        stage('Tag Docker Image') {
            steps {
                script {
                    // Tag Docker image for ECR
                     sh '''
                  docker tag devops-automation/main-image:latest 533267326662.dkr.ecr.us-east-1.amazonaws.com/new-one-testing-rep:devops-automation-main-image
                      '''
                   // sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${ECR_REPO_URI}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    // Push Docker image to ECR
                    sh '''
                 docker push 533267326662.dkr.ecr.us-east-1.amazonaws.com/new-one-testing-rep:devops-automation-main-image
                    //sh "docker push ${ECR_REPO_URI}/${IMAGE_NAME}:${IMAGE_TAG}"
                    '''
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    // Construct Docker image name and container name based on job name
                    def imageName = "${JOB_NAME}-image"
                    def containerName = "${JOB_NAME}-container"
                    
                    echo "Running Docker container: ${containerName}"
                    
                    // Run the Docker container from the built image
                    docker.image(imageName).run("-it -p 8081:8081 --name devops-automation")
                }
            }
        }

         }
    
    }    


