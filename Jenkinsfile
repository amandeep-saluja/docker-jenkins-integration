pipeline {
    agent any
    tools {
        maven 'MAVEN'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/amandeep-saluja/docker-jenkins-integration']])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t jacksparrow2023/docker-jenkins-integration .'
                }
            }
        }
        stage('Push image to Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {
                        sh 'docker login -u jacksparrow2023 -p ${dockerhub}'
                    }
                    sh 'docker push jacksparrow2023/docker-jenkins-integration'
                }
            }
        }
    }
}