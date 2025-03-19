pipeline {
    agent any

    environment {
        NODEJS_VERSION = '18' // Set the Node.js version
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/shyam9307/Angular-live-project'
            }
        }

        stage('Setup Node.js') {
            steps {
                script {
                    def nodeInstalled = sh(script: 'which node', returnStatus: true) == 0
                    if (!nodeInstalled) {
                        error "Node.js is not installed! Please install Node.js ${NODEJS_VERSION} on your Jenkins agent."
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build --prod'
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
            }
        }
    }

    post {
        success {
            echo "🎉 Build completed successfully!"
        }
        failure {
            echo "❌ Build failed. Check logs for errors."
        }
    }
}
