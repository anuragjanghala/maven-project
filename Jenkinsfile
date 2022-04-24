#!/usr/bin/env groovy

@Library('Jenkins-shared-library')_


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
        stage('build jar') {
            steps {
                script {
                    buildJar()
                }
            }
        }        
        stage('deploy') {
            steps {
                script {
                    buildImage()
                }
            }
        }
    }
}
