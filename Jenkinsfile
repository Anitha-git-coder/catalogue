pipeline {
    // these are prebuild section
    agent {
        node {
        label 'AGENT-1'
       }
    }
    environment { 
        COURSE = "Jenkins"
        appVersion = ""
    }
     options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 5, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
    
    // this is build section //
    stages {
        stage('ReadVersion') {
            steps {
                script{                        
                           def packageJSON = readJSON file: 'package.json'
                           appVersion = packageJSON.version
                           echo "app version: ${appVersion}"
                }
               
            }
        }
        stage('Install Dependencies') {
            steps {
                 script{
                        sh """ 
                         npm install
                        """
                }
            }
        }
        stage('Deploy') {
            //      input {
            //     message "Should we continue?"
            //     ok "Yes, we should."
            //     submitter "alice,bob"
            //     parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
            //     }
            // }

            when { 
                expression { "$params.DEPLOY" == "true" }
             }

            steps {
                 script{
                        sh """ 
                        echo "Deploying"
                        """
                }
            }
        }
    }

     post { 
        always { 
            echo 'I will always say Hello again!'
            cleanWs()
        }
        success{
                 echo 'its success!!!'   
        }
        failure{
                echo 'its failure !!!'
        }
       aborted {
                echo 'pipeline is aborted'
            }
    }


}
