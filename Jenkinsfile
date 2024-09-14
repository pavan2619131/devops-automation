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


//         stage('Run Docker Container') {
//     steps {
//         script {
//             // Construct Docker image name and container name based on job name
//             def imageName = "${JOB_NAME}-image"
//             // Replace slashes with hyphens to conform to Docker's naming rules
//             def containerName = "${JOB_NAME}-container".replace('/', '-')
            
//             echo "Running Docker container: ${containerName}"
            
//             // Run the Docker container from the built image
//             docker.image(imageName).run("-d -p 8080:8080 --name ${containerName}")
//         }
//     }
// }


        // stage('Run Docker Container') {
        //     steps {
        //         script {
        //             def imageName = "${JOB_NAME.replaceAll('/', '-')}-image"
        //             def containerName = "${JOB_NAME.replaceAll('/', '-')}-container"
                    
        //             echo "Running Docker container: ${containerName}"
                    
        //             // Run the Docker container from the built image
        //             sh "docker run -d -p 8080:8080 --name ${containerName} ${imageName}"
        //         }
        //     }
        // }



         }
    
    }    


