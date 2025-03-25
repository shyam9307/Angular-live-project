//New 

pipeline {
    agent any

    environment {
        BUILD_DIR = 'dist'
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
                sh 'npm run build'  // Run build without Angular CLI installation
                sh 'echo "Checking build output structure:"'
                sh 'ls -R'  // ✅ Print the full directory structure after build
            }
        }

        stage('Archive Build Artifacts') {
            steps {
                sh 'echo "Archiving artifacts from: ${BUILD_DIR}/"'
                archiveArtifacts artifacts: "${BUILD_DIR}/**/*", fingerprint: true
            }
        }
    }
}








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






// pipeline {
//     agent any

//     environment {
//         BUILD_DIR = 'dist'  // Updated from 'src/dist'
//         DEPLOY_DIR = '/var/www/html'
//         SERVER_USER = 'ec2-user'
//         SERVER_IP = '34.207.194.243'
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
//                 sh 'ls -R dist/'  // Debugging: Check if build output exists
//             }
//         }

//         stage('Archive Build Artifacts') {
//             steps {
//                 archiveArtifacts artifacts: "${BUILD_DIR}/**/*", fingerprint: true
//             }
//         }

//         stage('Deploy') {
//             steps {
//                 script {
//                     sh """
//                         echo "Deploying to server..."
//                         scp -r ${BUILD_DIR}/* ${SERVER_USER}@${SERVER_IP}:${DEPLOY_DIR}
//                     """
//                 }
//             }
//         }
//     }
// }








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
