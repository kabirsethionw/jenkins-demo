pipeline {
  agent any
  tools { maven 'maven-3.9' }   // configure in Manage Jenkins > Tools (auto-install)
  stages {   //triggers { githubPush() }
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build & Test') {
      steps { sh 'mvn -B clean package' }
    }
  }
  post {
    success { echo 'Build succeeded' }
    failure { echo 'Build failed' }
  }
}
