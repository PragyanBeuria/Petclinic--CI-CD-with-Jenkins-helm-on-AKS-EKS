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
                 def customImage = docker.build('"pragyan23/petcliniclab"', "./docker")
                 docker.withRegistry('mylab2024.azurecr.io', 'acr-demo') {
                 customImage.push("${env.BUILD_NUMBER}")
                 }                     
           }
        }
	  }
    }
}
