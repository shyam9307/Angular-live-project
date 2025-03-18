pipeline {
    agent {
        docker {
            image 'node:16'
            args '-u root'
        }
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    sh 'rm -rf Angular-Hello-World'
                    sh 'git clone https://github.com/shyam9307/Angular-live-project'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('Angular-Hello-World') {
                    sh 'npm install'
                }
            }
        }

        stage('Build Project') {
            steps {
                dir('Angular-Hello-World') {
                    sh 'npm run build -- --configuration=production'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Build completed successfully!"
        }
        failure {
            echo "❌ Build failed!"
        }
    }
}
