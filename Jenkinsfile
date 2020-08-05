pipeline {
    agent {
        label 'master'
    }
    tools {
        maven 'Maven'
    }
    environment {
        APP_NAME = "Sample_App"
        APP_VERSION = 5
    }
    parameters {
         booleanParam defaultValue: false, description: '', name: 'analysis'
    }
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '3', numToKeepStr: '10')
        disableConcurrentBuilds()
    }
    stages {
        stage('delete workspace') {
            steps {
                cleanWs()
            }
        }
        stage('clone') {
            steps {
                git credentialsId: 'ecd21cb2-43ed-4809-a473-fa2b10533e87', poll: false, url: 'https://github.com/githyma/learn-ci.git'
            }
        }
        
        stage('build') {
            steps {
                sh label: '', script: 'mvn clean install -DskipTests'
            }
        }
        stage('execute parallel') {
            parallel {
                stage('diplay AppName') {
                    steps {
                        sh 'echo $APP_NAME'
                            
                    }
                }
                stage('diplay AppVersion') {
                    steps {
                        
                        sh 'echo $APP_VERSION'         
                    }
                } 
            }
        }

        stage('code analysis'){
            when {
                expression {
                    params.analysis
                }
            }
            steps{
                sh 'mvn sonar:sonar'
            }
        }
        stage('diplay env') {
            steps {
                sh 'echo $APP_NAME'
                sh 'echo $APP_VERSION'         
             }
        }
        

    }
    post { 
        success {
           mail bcc: '', body: 'to send an email', cc: '', from: '', replyTo: '', subject: 'success', to: 'reachhyma4bi@gmail.com'
        }
        failure {
            mail bcc: '', body: 'to send an email', cc: '', from: '', replyTo: '', subject: 'failure', to: 'reachhyma4bi@gmail.com'
        }
        always {
            echo "job finished"
        }
    }
    
}
