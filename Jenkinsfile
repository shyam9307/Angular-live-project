pipeline {
    agent any

    environment {
        EC2_USER = 'ec2-user'
        EC2_IP = '54.211.194.46'
        EC2_KEY = '/var/lib/jenkins/.ssh/ec2-key.pem'
        BUILD_DIR = 'src/dist/'  // Update this if your build directory is different
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/shyam9307/Angular-live-project'
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
                sh 'ls -R src/dist/'  // Debugging: List build output
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: "${BUILD_DIR}**/*", fingerprint: true
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // Copy the build to EC2
                    sh """
                        scp -i ${EC2_KEY} -r ${BUILD_DIR} ${EC2_USER}@${EC2_IP}:/home/ec2-user/angular-app/
                    """

                    // SSH into EC2 and deploy the application
                    sh """
                        ssh -i ${EC2_KEY} ${EC2_USER}@${EC2_IP} <<EOF
                        # Install Nginx if not installed
                        sudo yum install -y nginx
                        
                        # Start and enable Nginx
                        sudo systemctl start nginx
                        sudo systemctl enable nginx
                        
                        # Remove old files and copy new ones
                        sudo rm -rf /usr/share/nginx/html/*
                        sudo cp -r /home/ec2-user/angular-app/* /usr/share/nginx/html/
                        
                        # Restart Nginx to apply changes
                        sudo systemctl restart nginx
                        EOF
                    """
                }
            }
        }
    }
}
