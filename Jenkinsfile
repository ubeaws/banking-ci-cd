pipeline {
  agent any

  tools {
    maven 'Maven 3.8.7'
    nodejs 'Node 18'
  }

  environment {
    SONARQUBE_SERVER = 'LocalSonar'
  }

  stages {
    stage('Checkout') {
      steps {
        git credentialsId: 'github-ssh', url: 'git@github.com:ubeaws/banking-ci-cd.git'
      }
    }

    stage('Build Backend') {
      steps {
        dir('backend') {
          sh 'mvn clean install'
        }
      }
    }

    stage('Build Frontend') {
      steps {
        dir('frontend') {
          sh 'npm install || true'
          sh 'npm run build'
        }
      }
    }

    stage('Unit Tests') {
      steps {
        dir('backend') {
          sh 'mvn test'
        }
      }
    }

stage('SonarQube Analysis') {
  steps {
    withSonarQubeEnv('MySonar') { // replace 'MySonar' with your configured server name
      dir('backend') {
        sh 'mvn clean verify sonar:sonar'
          }
        }
      }
    }
  }
}

