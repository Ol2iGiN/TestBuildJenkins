def remote = [:]
remote.name = 'root'
remote.host = '188.166.233.163'
remote.allowAnyHosts = true

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
        sh 'cd /root/workspace'
        sh 'ls -l'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}