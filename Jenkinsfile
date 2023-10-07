
pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {
    stage('checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'docker build -t devilpk/jenkins-docker-hub .'
      }
    }
    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push devilpk/jenkins-docker-hub'
      }
    }
    stage('Deploy'){
      steps{
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:'DOIkhody', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
          sh 'ssh root@143.198.86.97 "commands to execute"'
          sh 'ls -l'
        }
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}