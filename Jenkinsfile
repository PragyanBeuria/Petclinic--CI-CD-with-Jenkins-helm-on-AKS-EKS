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
        
        stage('Build and Push Docker Image') {
            environment {
                DOCKER_REGISTRY = 'https://mylab2024.azurecr.io'
            }
            steps {
                script {
                    docker.withRegistry("${env.DOCKER_REGISTRY}") {
                        docker.build('pragyan23/petcliniclab').push()
                    }
                }
            }
        }
    }
}


