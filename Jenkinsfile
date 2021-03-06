#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        maven 'maven-3.8.5'
    }
    stages {
        stage('increment version') {
            when {
                expression {
                    BRANCH_NAME == 'jenkins-jobs'
                }
            }
            steps {
                script {
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
        stage('build app') {
            when {
                expression {
                    BRANCH_NAME == 'jenkins-jobs'
                }
            }
            steps {
                script {
                    echo "building the application...."
                    sh 'mvn clean package'
                }
            }
        }
        stage('build image') {
            when {
                expression {
                    BRANCH_NAME == 'jenkins-jobs'
                }
            }
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-pri-repo-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh "docker build -t ajanghala/my-private-repo:${IMAGE_NAME} ."
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push ajanghala/my-private-repo:${IMAGE_NAME}"
                    }
                }
            }
        }
        stage('deploy') {
            when {
                expression {
                    BRANCH_NAME == 'jenkins-jobs'
                }
            }
            steps {
                script {
                    echo 'deploying docker image to EC2....'
                    // def dockerCmd = "docker run -p 8080:8080 -d ${IMAGE_NAME}"
                    def shellCmd = "bash ./server-cmds.sh ajanghala/my-private-repo:${IMAGE_NAME}"
                    def ec2Instance = "ec2-user@34.244.117.149"

                    sshagent(['ec2-server-key']) {
                        sh "scp server-cmds.sh ${ec2Instance}:/home/ec2-user"
                        sh "scp docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} ${shellCmd}"
                    }
                }
            }
        }
        stage('commit version update') {
            when {
                expression {
                    BRANCH_NAME == 'jenkins-jobs'
                }
            }
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        // git config here for the first time run
                        sh "git remote set-url origin https://${USER}:${PASS}@github.com/anuragjanghala/maven-project.git"
                        sh 'git add .'
                        sh 'git commit -m "ci: version bump"'
                        sh 'git push origin HEAD:jenkins-jobs'
                    }
                }
            }
        }
    }
}
