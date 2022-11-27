pipeline {
  agent any
    tools {
      maven 'MAVEN_3.8'
      jdk 'JDK17'
    }
    stages {      
        stage('Build maven image') {
            steps { 
                    sh 'pwd'      
                    sh 'mvn clean install package'
            }
        }
        
        stage('Copy Artifact from Target to Docker') {
           steps { 
                   sh 'pwd'
		   sh 'cp -r target/*.jar docker'
           }
        }
         
        stage('Push to Docker image') {
           steps {
               script {         
                 def customImage = docker.build('initsixcloud/petclinic', "./docker")
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                 customImage.push("${env.BUILD_NUMBER}")
                 }                     
           }
        }
	  }
    }
}
