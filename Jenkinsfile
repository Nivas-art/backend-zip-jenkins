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
                def project = readJSON file: "Projects.json"
                appversion = project.version
                echo "appversion is: ${appversion}"
               }
            }
        } 
    }

     stages {
        stage('dependece insatll') {
            steps {
               sh """
                npm insatll
                ls -ltr
                echo "appversion is: ${appversion}"
               """
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
}