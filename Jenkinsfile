// node{
//     def appdir = '/var/www/html'
//     stage('cleaning'){
//         echo 'workspace is cleaning'
//         deleteDir()
//     }
//     stage('clone repo'){
//         echo 'cloning repo'
//         git(
//             branch: 'main',
//             url: 'https://github.com/aman90909090/jenkins_test1.git'
//         )
//     }
//     stage('deploy to ec2 '){

//         echo 'deploying to ec2'
//         sh """
//           sudo mkdi


pipeline {
    agent any

    environment {
        DEPLOY_PATH = '/var/www/html'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // GitHub repo se latest code pull karega
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing npm dependencies...'
                sh 'npm install'
            }
        }

        stage('Build Vite App') {
            steps {
                echo 'Building React Vite application...'
                // Vite production build create karega (dist folder)
                sh 'npm run build'
            }
        }

        stage('Deploy to Nginx') {
            steps {
                echo 'Deploying build files to Nginx web root...'
                sh '''
                    # Purani files clear karke naya build copy karein
                    rm -rf ${DEPLOY_PATH}/*
                    cp -r dist/* ${DEPLOY_PATH}/
                '''
            }
        }

        stage('Reload Nginx') {
            steps {
                echo 'Reloading Nginx server...'
                sh 'sudo systemctl reload nginx || sudo service nginx reload'
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful! App is live.'
        }
        failure {
            echo 'Deployment Failed. Check logs above.'
        }
    }
}
