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
// }
node {
    def appdir = '/var/www/html'
    
    stage('Cleaning') {
        echo 'Cleaning workspace...'
        deleteDir()
    }
    
    stage('Clone Repo') {
        echo 'Cloning repository...'
        git(
            branch: 'main',
            url: 'https://github.com/aman90909090/jenkins_test1.git'
        )
    }
    
    stage('Deploy to EC2') {
        echo 'Deploying to EC2...'
        
        // Use sshagent if deploying to a remote EC2, 
        // or keep as sh if Jenkins runs directly on the target EC2.
        sh """
            sudo mkdir -p ${appdir}
            sudo chown -R jenkins:jenkins ${appdir}
            
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appdir}
            
            cd ${appdir}
            npm install
            npm run build
            
            # Start/Restart the app in the background using PM2
            pm2 restart my-node-app || pm2 start npm --name "my-node-app" -- run start
        """
    }
}
