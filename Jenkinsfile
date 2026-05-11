pipeline {

    agent {
        node {
            label 'AGENT-1'
        }
    }

    environment {
        COURSE = "Jenkins"
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
                sh """
                docker build -t catalogue:${env.appVersion} .
                docker images
                """
            }
        }

        stage('Deploy') {

            when {
                expression { params.DEPLOY }
            }

            steps {
                sh '''
                echo "Deploying"
                '''
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