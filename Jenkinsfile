pipeline {
    agent any

    environment {
        // Define the key and EC2 IP address as environment variables
        EC2_KEY = '/var/lib/jenkins/.ssh/ec2-key.pem'
        EC2_USER = 'ec2-user'
        EC2_IP = '54.211.194.46'
        BUILD_DIR = 'src/dist'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/shyam9307/Angular-live-project'
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
                sh 'ls -R ${BUILD_DIR}/' // Verify files are generated
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: "${BUILD_DIR}/**/*", fingerprint: true
            }
        }

        stage('Deploy to EC2') {
            steps {
                script {
                    // Copy the build to EC2
                    sh """
                        scp -i ${EC2_KEY} -r ${BUILD_DIR}/ ec2-user@${EC2_IP}:/home/ec2-user/angular-app/
                    """

                    // SSH into EC2 and deploy
                    sh """
                        ssh -i ${EC2_KEY} ec2-user@${EC2_IP} << 'EOF'
                            # Install Nginx if not installed
                            sudo yum install -y nginx
                            
                            # Start Nginx if it's not running
                            sudo systemctl start nginx
                            sudo systemctl enable nginx

                            # Copy build files to Nginx public directory
                            sudo rm -rf /usr/share/nginx/html/*
                            sudo cp -r /home/ec2-user/angular-app/* /usr/share/nginx/html/

                            # Restart Nginx to serve the new files
                            sudo systemctl restart nginx
                        EOF
                    """
                }
            }
        }
    }
}
