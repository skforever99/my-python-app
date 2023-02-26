#!/usr/bin/env groovy

pipeline {
  agent any
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('Build') 
    {
      steps {
        sh 'docker build -t skforever99/python-app:latest1 .'          # building docker image from docker file
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin' # giving credentials to jenkins for docker hub
      }
    }
    stage('Push') {
      steps {
        sh 'docker push skforever99/python-app:latest1'                     #pushing our build image to docker hub
      }
    }
     stage ('pull'){
      steps { 
            sh 'sudo docker pull skforever99/python-app:latest1 '             # jenkins pulling image from docker hub to deploy 
           } 
    }
    stage ('deploy'){
      steps {  sh 'sudo docker run -d -p 80:80 skforever99/python-app:latest1' }          #deploying docker image at port 80
    }
  }
  post {
    always {
      sh 'docker logout'
    }
    
   
  }
}