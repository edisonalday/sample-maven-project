pipeline {
    agent any
    tools {
        maven 'Maven'
        jdk 'Java'
    }
    stages{
        stage('Package Application'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Application code successfully packaged.'
                }
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
