pipeline {
  agent any
  stages {
    stage('Front-end') {
      agent {
        docker { 
          image 'node:16-alpine' 
          args '--user root'  // Run as root inside the container to avoid permission issues
        }
      }
      environment {
        NPM_CONFIG_CACHE = "$WORKSPACE/.npm"  // Set npm cache directory to a writable location
      }
      steps {
        script {
          sh 'mkdir -p $NPM_CONFIG_CACHE'  // Ensure the directory exists
          sh 'npm install --unsafe-perm'   // Allow installing packages with root
          sh 'nohup npm start &'           // Run npm start in the background
        }
      }
    }
  }
  post {
    success {
      script {
        def baseUrl = "http://your-server-address"  // Change this to your actual server address
        def od = "${baseUrl}/od"
        def ci = "${baseUrl}/ci"
        def pr = "${baseUrl}/pr"

        echo "Successfully executed the pipeline. Here are the links:"
        echo "OD Link: ${od}"
        echo "CI Link: ${ci}"
        echo "PR Link: ${pr}"
      }
    }
    failure {
      echo "Pipeline execution failed."
    }
  }
}

