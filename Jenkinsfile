pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    environment{
       def appversion = ''
    }
    
    stages {
        stage('read the version') {
            steps {
               script{
                def project = readJSON file: 'package.json'
                appversion = project.version
                echo "appversion is: $appversion"
               }
            }
        } 

        stage('dependece insatll') {
            steps {
               sh """
                npm install
                ls -ltr
                echo "appversion is: $appversion"
               """
            }
        }
           stage('sonar scane') {
            environment{
                scannerHome = tool 'sonar-6.2'
            }
            steps{
                script{
                    withSonarQubeEnv('sonar-6.2'){
                        sh "${scannerHome}/bin/sonar-scanner"
                }
                }
            }
        } 

        stage('zip the file') {
            steps {
                sh """
                zip -q -r backend-${appversion}.zip * -x Jenkinsfile -x backend-${appversion}.zip
                ls -ltr
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
            echo 'I will run when pipeline is success'
        }
        failure { 
            echo 'I will run when pipeline is failure'
        }
    }
}