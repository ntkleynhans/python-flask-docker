pipeline {
  environment {
    imagename = "neilspand/flask-signup"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  options {
    disableConcurrentBuilds()  
  }
  stages {
    stage('Cloning Git') {
      steps {
        checkout scm
      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("v_$BUILD_NUMBER")
          }
        }
      }
    }
    stage('Update Service') {
      steps{
        sh "/home/ec2-user/jenkins/scripts/aws_update_service.sh $BUILD_NUMBER"
      }
    }
  }
}
