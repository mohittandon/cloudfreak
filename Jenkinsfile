pipeline {
  agent any
    tools {
      maven 'maven3'
                 jdk 'JDK8'
    }
    stages {      
        stage('Build maven ') {
            steps { 
                  sh 'pwd'      
            sh 'mvn  clean install package'
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
              withCredentials([string(credentialsId: 'dockerhub_password', variable: 'Dockerhub_pass')]) {
                sh "docker login -u mohittndn -p ${dockerhub_password}"
		def customImage = docker.build('mohittndn/petclinic', "./docker")
                docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                customImage.push("${env.BUILD_NUMBER}")
                }
              }    
            }
          }
        }
  }
}
