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
        git url: 'https://github.com/ubeaws/banking-ci-cd.git'
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
          sh 'npm install && npm run build'
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
        withSonarQubeEnv("${SONARQUBE_SERVER}") {
          dir('backend') {
            sh 'mvn sonar:sonar'
          }
        }
      }
    }
  }
}

