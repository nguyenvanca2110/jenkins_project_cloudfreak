pipeline {
  agent any
    tools {
      maven 'MAVEN_3.8'
      jdk 'JDK17'
    }

    environment {
      DOCKERHUB_CREDENTIALS=credentials('dockerhub_canv2110')
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


		    stage('Build') {
          steps {
            sh 'docker build -t bharathirajatut/nodeapp:latest .'
          }
        }

        stage('Login') {
          steps {
            sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
          }
          		}

		stage('Push') {

			steps {
				sh 'docker push bharathirajatut/nodeapp:latest'
			}
		}

	post {
		always {
			sh 'docker logout'
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
