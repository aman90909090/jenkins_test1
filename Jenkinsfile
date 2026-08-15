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
//           sudo mkdir -p ${appdir}
//           sudo chown -R jenkins:jenkins ${appdir}

//           rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appdir}
//         cd ${appdir}
//         sudo npm install
//         sudo npm run build
//         sudo fuser -k 3000/tcp || true
//         npm run start
//         """
//     }

node {
    // Application deployment directory path
    def appdir = '/var/www/html'

    stage('Cleaning') {
        echo 'Workspace is cleaning...'
        deleteDir()
    }

    stage('Clone Repo') {
        echo 'Cloning repository...'
        git(
            branch: 'main',
            url: 'https://github.com'
        )
    }

    stage('Deploy to EC2') {
        echo 'Deploying to EC2...'
        sh """
            # 1. Directory create karna aur permissions setup karna
            sudo mkdir -p ${appdir}
            sudo chown -R jenkins:jenkins ${appdir}

            # 2. Files ko deploy folder me sync karna
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appdir}

            # 3. Application folder me jana
            cd ${appdir}

            # 4. Dependencies install karna aur build banana (Sudo ki zaroorat nahi hai)
            npm install
            npm run build

            # 5. Application ko background me PM2 ke sath chalana (Taaki Jenkins pipeline hang na ho)
            pm2 restart "my-node-app" || pm2 start npm --name "my-node-app" -- run start
        """
    }
}

            
            # Start/Restart the app in the background using PM2
            pm2 restart my-node-app || pm2 start npm --name "my-node-app" -- run start
        """  
    }
}
