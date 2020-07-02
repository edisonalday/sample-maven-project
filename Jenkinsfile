pipeline {
    agent any
    stages{
        stage('Package Application'){
            steps {
                sh 'mvn clean package'
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
                    input message:'Application deployment to  Nexus approved?'
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
