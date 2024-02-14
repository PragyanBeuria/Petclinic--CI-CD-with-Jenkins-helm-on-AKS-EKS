pipeline {
    agent any
    tools {
        maven 'Maven'
        jdk 'JDK11'
    }
    stages {      
        stage('Build maven') {
            steps { 
                sh 'pwd'      
                sh 'mvn clean install package'
            }
        }
        
        stage('Copy Artifact') {
            steps { 
                sh 'pwd'
                sh 'cp -r target/*.jar docker'
            }
        }
         
        stage('Build docker image') {
            steps {
                script {
                    def docker = tool 'docker'  // Ensure Docker is configured in Jenkins
                    
                    // Build the Docker image
                    def customImage = docker.build('pragyan23/petcliniclab', "./docker")
                    
                    // Push the image to Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                        customImage.push("${env.BUILD_NUMBER}")
                    }                     
                }
            }
        }
    }
}
