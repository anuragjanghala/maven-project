#!/usr/bin/env groovy

pipeline {
    agent none
    parameters {
        choice(name: 'VERSION', choices: ['1.1.0','1.2.0','1.3.0'], description: '')
        booleanParam(name: 'executeTests', defaultValue: true, description: '')
    }
    stages {
        stage('build') {
            steps {
                script {
                    echo "Building the application..."
                }
            }
        }
        stage('test') {
            steps {
                script {
                    echo "Testing the application..."
                }
            }
        }
        stage('deploy') {
            input {
                message "Select the environment to deplot to"
                ok "Done"
                parameters {
                    choice(name: 'ONE', choices: ['dev','staging','prod'], description: '')
                    choice(name: 'TWO', choices: ['dev','staging','prod'], description: '')
                }
            }
            steps {
                script {
                    echo "Deploying the application..."
                    echo "Deploying to ${ONE} version ${params.VERSION}"
                    echo "Deploying to ${TWO} version ${params.VERSION}"
                }
            }
        }
    }
}
