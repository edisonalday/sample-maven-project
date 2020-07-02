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
        stage('Approval'){
            steps {
                timeout(time:5, unit:'DAYS'){
                    input message: 'Approve package?', ok: 'Approve', submitter: 'Edison Alday'
             }
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
        stage ('Deploy to Nexus'){
            steps {
                bat 'mvn clean package deploy'
            }
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'maven-project', classifier: '', file: 'webapp/target/webapp.war', type: 'war']], credentialsId: 'nexusdeploymentrepo', groupId: 'com.example.maven-project', nexusUrl: '192.168.101.66:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOT'
        }
    }
}
