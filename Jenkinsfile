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
                timeout(time: 5, unit: 'MINUTES') {
                    dir('Angular-Hello-World') {
                        sh '''
                        set -e
                        npm ci
                        '''
                    }
                }
            }
        }
        stage('Build Project') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    dir('Angular-Hello-World') {
                        sh '''
                        set -e
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
