#!/usr/bin/env groovy

pipeline {
    agent none
    stages {
        stage('test') {
            steps {
                script {
                    echo "Testing the application..."
                    echo "Testing the branch $BRANCH_NAME"
                }
            }
        }
        stage('build') {
            steps {
                script {
                    echo "Building the application..."
                    echo "testing trigger"
                }
            }
        }        
        stage('deploy') {
            steps {
                script {
                    def dockerCmd = 'docker run -p 3080:80 -d ajanghala/my-private-repo:1.0'
                    sshagent(['ec2-server-key']) {
                        sh "ssh -o StrictHostKeyChecking=no ec2-user@34.244.117.149 ${dockerCmd}"
                    }
                }
            }
        }
        }
    }
}
