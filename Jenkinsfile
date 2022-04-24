#!/usr/bin/env groovy

@Library('Jenkins-shared-library')_


pipeline {
    agent any
    tools {
        maven 'maven-3.8.5'
    }
    stages {
        stage('build jar') {
            steps {
                script {
                    buildJar()
                }
            }
        }
        stage('build image') {
            steps {
                script {
                    buildImage('ajanghala/my-private-repo:3.1')

                }
            }
        }
        stage('deploy') {
            steps {
                script {
                    echo "Testing the application..."
                    echo "Testing the branch $BRANCH_NAME"
                }
            }
        }
    }
}
