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
        nexusUrl = 'nexus.hemanth78s.online:8081'
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
        stage('Build'){
            steps{
                sh """
                zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
                ls -ltr

                """
            }
        }
        stage('Sonar scan'){
            environment {
                scannerHome = tool 'Sonar-6.0' // referring scanner CLI
            }
            steps {
                script {
                    withSonarQubeEnv('Sonar-6.0') {  // referring sonar server 
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=<project-key> \
                            -Dsonar.projectName=<project-name> \
                            -Dsonar.projectVersion=<project-version> \
                            -Dsonar.sources=<project-path>"
                    }
                }
            }
            
        }
        stage('Nexus Artifact Upload'){
            steps{
                script{
                     nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${nexusUrl}",
                        groupId: 'com.expense',
                        version: "${appVersion}",
                        repository: "backend",
                        credentialsId: 'nexus-auth',
                        artifacts: [
                            [artifactId: "backend",
                            classifier: '',
                            file: "backend-" + "${appVersion}" + '.zip',
                            type: 'zip']
                        ]
                    )
                }
            }
        }
        stage('Deploy'){
            steps{
                
                script{
                    def params = [
                    string(name: 'appVersion', value: "${appVersion}")
                    ]
                    build job: 'backend-deploy', parameters: params, wait: false
                }
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