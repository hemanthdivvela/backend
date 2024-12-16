pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    
    

    stages {
        stage('read the version'){
            steps {
                def PackageJson = readJson file: 'package.json'
                def appVersion = PackageJson.version
                echo "application version: $appVersion"
            }

        }
        stage('npm dependices') {
            steps {
                sh """
                 npm install
                 ls -ltr
                 echo "application version: $appVersion"
                """
            }
        }
        
    }
    post { 
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'I will run when pipline is success!'
        }
        failure { 
            echo 'I will run when pipline is failure!'
        }
    }
}   