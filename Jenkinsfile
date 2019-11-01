pipeline {
  
  environment {
    registry = "nevincleetus/java-web-app-cicd"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        git  'https://github.com/nevincleetus/java-webapp-cicd.git'
      }
    }
    stage('Compile Webapp'){
      //agent {
	//docker {
        //   image 'maven:3-alpine'
        //   args '-v /root/.m2:/root/.m2'
        //}
      //}
      //steps {
      //   sh 'mvn -B -DskipTests clean package'
      //} 
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }
  }
}
