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
            when {
                expression {
                    BRANCH_NAME == 'main'
                }
            }
            steps {
                script {
                    echo "Deploying the application..."
                }
            }
        }
    }
}
