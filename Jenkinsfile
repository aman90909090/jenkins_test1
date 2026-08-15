node{
    def appdir = '/var/www/html'
    stage('cleaning'){
        echo 'workspace is cleaning'
        deleteDir()
    }
    stage('clone repo'){
        echo 'cloning repo'
        git(
            branch: 'main',
            url: 'https://github.com/aman90909090/jenkins_test1.git'
        )
    }
    stage('deploy to ec2 '){

        echo 'deploying to ec2'
        sh """
          sudo mkdir -p ${appdir}
          sudo chown -R jenkins:jenkins ${appdir}

          rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appdir}
        cd ${appdir}
        sudo npm install
        sudo npm run build
        sudo fuser -k 3000/tcp || true
        npm run start
        """
    }
}