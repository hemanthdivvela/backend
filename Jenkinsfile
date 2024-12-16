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
        stage('npm dependices') {
            steps {
                sh """
                 npm install
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