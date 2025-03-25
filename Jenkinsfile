//Build Jenkinsfile: 

// pipeline {
//     agent any

//     environment {
//         PATH = "/usr/bin:$PATH"  // Ensure npm is accessible
//         BUILD_DIR = 'dist'       // Expected output folder for build artifacts
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git branch: 'master', url: 'https://github.com/shyam9307/Angular-live-project'
//             }
//         }
//         stage('Install Dependencies') {
//             steps {
//                 // Print Node and npm versions for debugging
//                 sh 'node -v'
//                 sh 'npm -v'
//                 // Clean up previous installs
//                 sh 'rm -rf node_modules package-lock.json || true'
//                 sh 'npm cache clean --force'
//                 // Install dependencies freshly
//                 sh 'npm install'
//             }
//         }
//         stage('Build Angular App') {
//             steps {
//                 // Build the Angular app with output directed to BUILD_DIR
//                 sh 'npm run build -- --output-path=dist'
//                 // List the output directory to verify build artifacts
//                 sh 'ls -l ${BUILD_DIR}/'
//             }
//         }
//         stage('Archive Build Artifacts') {
//             steps {
//                 archiveArtifacts artifacts: "${BUILD_DIR}/**/*", fingerprint: true
//             }
//         }
//     }
// }






//Deploy Jenkinsfile:

//Jenkinsfile:

pipeline {
    agent any

    environment {
        PATH = "/usr/bin:$PATH"           // Ensure npm is accessible
        BUILD_DIR = 'dist'                // Output folder for build artifacts
        EC2_USER = "ec2-user"
        EC2_IP = "3.111.135.252"          // Replace with your EC2 public IP
        REMOTE_DIR = "/usr/share/nginx/html" // Change if your web root is different
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/shyam9307/Angular-live-project'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'node -v'
                sh 'npm -v'
                sh 'rm -rf node_modules package-lock.json || true'
                sh 'npm cache clean --force'
                sh 'npm install'
            }
        }
        stage('Build Angular App') {
            steps {
                sh 'npm run build -- --output-path=dist'
                sh 'ls -l ${BUILD_DIR}/'
            }
        }
        stage('Create Environment Files') {
            steps {
                sh 'echo "Operations Deployment" > od'
                sh 'echo "Continuous Integration" > ci'
                sh 'echo "Pull Request" > pr'
                sh 'ls -l'
            }
        }
        stage('Archive Build Artifacts') {
            steps {
                archiveArtifacts artifacts: "${BUILD_DIR}/**/*, od, ci, pr", fingerprint: true
            }
        }
        stage('Deploy to EC2') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'EC2_SSH_KEY', keyFileVariable: 'SSH_KEY_PATH', usernameVariable: 'SSH_USER')]) {
                    script {
                        def deployCmd = """
                        ssh -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} ${EC2_USER}@${EC2_IP} '
                            sudo rm -rf ${REMOTE_DIR}/* &&
                            sudo mkdir -p ${REMOTE_DIR} &&
                            exit
                        '
                        """
                        sh deployCmd
                    }
                    sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} -r ${BUILD_DIR}/* ${EC2_USER}@${EC2_IP}:${REMOTE_DIR}/"
                    sh "scp -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} od ci pr ${EC2_USER}@${EC2_IP}:${REMOTE_DIR}/"
                    
                    // Restart Nginx
                    sh "ssh -o StrictHostKeyChecking=no -i ${SSH_KEY_PATH} ${EC2_USER}@${EC2_IP} 'sudo systemctl restart nginx'"
                }
            }
        }
    }
}
