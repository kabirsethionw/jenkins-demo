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
    stage('Docker Build') {
      steps { sh 'docker build -t jenkins-demo:${BUILD_NUMBER} .' }
    }
    stage('Deploy') {
      when { branch 'main' }
      steps {
        withCredentials([string(credentialsId: 'demo-secret', variable: 'APP_SECRET')]) {
          sh '''
            docker rm -f jenkins-demo || true
            docker run -d --name jenkins-demo -e APP_SECRET=$APP_SECRET -p 8081:8080 jenkins-demo:${BUILD_NUMBER}
          '''
        }
      }
    }
  }
  post {
    success { echo 'Build succeeded' }
    failure { echo 'Build failed' }
  }
}
