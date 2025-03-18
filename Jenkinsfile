pipeline {
    agent {
        docker {
            image 'node:16'
            args '-u root'
        }
    }
    environment {
        NODE_OPTIONS = "--max_old_space_size=4096"  // Prevents memory issues during build
    }
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    sh '''
                    set -e
                    if [ ! -d "Angular-Hello-World" ]; then
                        git clone https://github.com/shyam9307/Angular-live-project Angular-Hello-World
                    fi
                    '''
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                timeout(time: 3, unit: 'MINUTES') {
                    dir('Angular-Hello-World') {
                        sh '''
                        set -e
                        echo "🔄 Cleaning npm cache..."
                        npm cache clean --force
                        
                        echo "🧹 Removing node_modules..."
                        rm -rf node_modules
                        
                        if [ -f package-lock.json ]; then
                            echo "📦 Installing dependencies using npm ci..."
                            npm ci
                        else
                            echo "⚠️ package-lock.json not found. Running npm install instead..."
                            npm install
                        fi
                        '''
                    }
                }
            }
        }
        stage('Build Project') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    dir('Angular-Hello-World') {
                        sh '''
                        set -e
                        echo "🏗️ Building the project..."
                        npm run build -- --configuration=production
                        '''
                    }
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
            script {
                currentBuild.result = 'FAILURE'
            }
        }
    }
}
