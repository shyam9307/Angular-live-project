pipeline {
  agent none
  stages {
    stage('Front-end') {
      agent {
        docker { image 'node:16-alpine' }
      }
      steps {
        script {
          sh 'npm install' // Step 1: Install dependencies
        }
      }
      steps {
        script {
          sh 'npm start' // Step 2: Start the Angular app
        }
      }
    }
  }
}

