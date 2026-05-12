pipeline {

    agent {
        node {
            label 'AGENT-1'
        }
    }

    environment {
        COURSE = "Jenkins"
        appVersion = ""
        ACC_ID = "136337412157"
        PROJECT = "roboshop"
        COMPONENT = "catalogue"
    }

    options {
        timeout(time: 5, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    parameters {
        booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Deploy application')
    }

    stages {

        stage('ReadVersion') {
            steps {
                script {
                    def packageJSON = readJSON file: 'package.json'
                    env.appVersion = packageJSON.version
                    echo "app version: ${env.appVersion}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                npm install
                '''
            }
        }

       stage('Image Build') {
       steps {
        script {
            withAWS(region:'us-east-1', credentials:'aws-creds') {
                sh """
                aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${env.appVersion} .

                docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${env.appVersion}
                """
            }
        }
    }
}

      
    }

    post {

        always {
            echo 'I will always say Hello again!'
            cleanWs()
        }

        success {
            echo 'its success!!!'
        }

        failure {
            echo 'its failure !!!'
        }

        aborted {
            echo 'pipeline is aborted'
        }
    }
}