pipeline {
    agent any
    tools {
        maven 'Maven'
        jdk 'Java'
    }
    stages{
        stage('Code Validation'){
            steps {
                bat 'mvn clean validate'
            }
        }
        stage('Code Compilation'){
            steps {
                bat 'mvn clean compile'
            }
        }
        stage('Unit Test'){
            steps {
                bat 'mvn clean test'
            }
        }
        stage('Package Application'){
            steps {
                bat 'mvn clean package'
            }
        }
        stage ('Deploy to Nexus'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message: 'Approve deployment to Nexus?', ok: 'Approve', parameters: [choice(choices: ['Approved, Not Approved'], description: '', name: '')], submitter: 'Edison Alday'
                }
                build job: 'deploy-to-nexus'
            }
            post {
                success {
                    echo 'Packaged application code deployment to Nexus approved.'
                }

                failure {
                    echo 'Packaged application code deployment to Nexus failed - please visit test result.'
                }
            }
        }
    }
}
