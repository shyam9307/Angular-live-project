pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git 'your-repository-url'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build'
                sh 'ls -R src/dist/' // Debugging: Verify if files are generated
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: 'src/dist/**/*', fingerprint: true
            }
        }
    }
}
