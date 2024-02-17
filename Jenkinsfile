pipeline {
    agent any
    environment {
        DOCKER_REGISTRY = 'mylab2024.azurecr.io'
    }
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

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build('-t pragyan23/petcliniclab -f docker/Dockerfile .')

                    // Log in to Azure Container Registry
                    withCredentials([azureServicePrincipal(credentialsId: 'acr-demo', variable: 'AZURE_CREDENTIALS')]) {
                        sh "az acr login --name mylab2024"
                    }

                    // Tag the Docker image for ACR
                    docker.image('pragyan23/petcliniclab').tag("${env.DOCKER_REGISTRY}/pragyan23/petcliniclab:latest")

                    // Push the Docker image to Azure Container Registry
                    docker.withRegistry("${env.DOCKER_REGISTRY}", 'azure') {
                        docker.image("${env.DOCKER_REGISTRY}/pragyan23/petcliniclab:latest").push()
                    }
                }
            }
        }
    }
}

                
