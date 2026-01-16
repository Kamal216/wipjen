pipeline {
  agent any
  tools {
    jdk 'Java21'
    maven 'Maven'
  }
  stages {
    stage('Checkout Code') {
      steps {
        echo 'Pulling from Github'
        git branch: 'main', credentialsId: 'gitcred', url: 'https://github.com/Kamal216/wipjen.git'
      }
    }
    stage('Test Code') {
      steps {
        echo 'JUNIT Test case execution started'
        bat 'mvn clean test'
        
      }
      post {
        always {
		  junit '**/target/surefire-reports/*.xml'
          echo 'Test Run is SUCCESSFUL!'
        }

      }
    }
    stage('Build Project') {
      steps {
        echo 'Building Java project'
        bat 'mvn clean package -DskipTests'
      }
    }
    stage('Build the Docker Image') {
      steps {
        echo 'Building Docker Image'
        bat 'docker build -t indiaproj-1.0 .'
      }
    }
    
    stage('Run Docker Container') {
      steps {
        echo 'Running Java Application'
        bat '''
        docker rm -f indiaproj-container || exit 0
        docker run --name indiaproj-container indiaproj-1.0
        
        '''               
      }
    }
  }
  post {
    success {
      echo 'BUild and Run is SUCCESSFUL!'
    }
    failure {
      echo 'OOPS!!! Failure.'
    }
  }
}