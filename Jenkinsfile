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
    environment{
        def appVersion = '' //variable declartion in gobal
    }
       
    stages {
        stage('read the version'){
            steps {
                script{

                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "application version: $appVersion"
                }
            }

        }
        stage('install Dependencies') {
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