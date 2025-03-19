pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'ng build --prod'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['ec2-ssh-key']) {  // Use the credential ID from Step 2
                    sh """
                        ssh -o StrictHostKeyChecking=no ec2-user@your-ec2-public-ip <<EOF
                        sudo rm -rf /var/www/html/angular-app/*
                        sudo cp -r dist/angular-app/* /var/www/html/angular-app/
                        sudo systemctl restart nginx
                        EOF
                    """
                }
            }
        }
    }
}






// To Build Script:

// pipeline {
//     agent any

//     environment {
//         EC2_USER = 'ec2-user'
//         EC2_IP   = '54.211.194.46'
//         EC2_KEY  = '/var/lib/jenkins/.ssh/ec2-key.pem'
//         BUILD_DIR = 'src/dist'
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git branch: 'master', url: 'https://github.com/shyam9307/Angular-live-project'
//             }
//         }

//         stage('Install Dependencies') {
//             steps {
//                 sh 'npm install'
//             }
//         }

//         stage('Build Angular App') {
//             steps {
//                 sh 'npm run build'
//                 sh 'ls -R src/dist/'  // Debug: List build output
//             }
//         }

//         stage('Archive Build Artifacts') {
//             steps {
//                 archiveArtifacts artifacts: "${BUILD_DIR}/**/*", fingerprint: true
//             }
//         }

//         stage('Deploy to EC2') {
//             steps {
//                 script {
//                     // Copy the build directory to EC2
//                     sh """
//                         scp -i ${EC2_KEY} -r ${BUILD_DIR}/ ${EC2_USER}@${EC2_IP}:/home/ec2-user/angular-app/
//                     """

//                     // SSH into EC2 and execute deployment commands using a heredoc.
//                     sh '''#!/bin/bash
// ssh -i ${EC2_KEY} ${EC2_USER}@${EC2_IP} << "EOF"
// sudo yum install -y nginx
// sudo systemctl start nginx
// sudo systemctl enable nginx
// sudo rm -rf /usr/share/nginx/html/*
// sudo cp -r /home/ec2-user/angular-app/* /usr/share/nginx/html/
// sudo systemctl restart nginx
// EOF
// '''
//                 }
//             }
//         }
//     }
// }





// Only Build Script:

// pipeline {
//     agent any

//     environment {
//         BUILD_DIR = 'src/dist'
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git branch: 'master', url: 'https://github.com/shyam9307/Angular-live-project'
//             }
//         }

//         stage('Install Dependencies') {
//             steps {
//                 sh 'npm install'
//             }
//         }

//         stage('Build Angular App') {
//             steps {
//                 sh 'npm run build'
//                 sh 'ls -R src/dist/'  // Debug: List build output
//             }
//         }

//         stage('Archive Build Artifacts') {
//             steps {
//                 archiveArtifacts artifacts: "${BUILD_DIR}/**/*", fingerprint: true
//             }
//         }
//     }
// }
