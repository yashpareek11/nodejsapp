pipeline {
    agent any
    tools {nodejs "node"}

    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/yashpareek11/nodejsapp.git']])
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('docker build image') {
            steps {
                sh 'docker build -t yashpareek99/nodejsapp .'
            }
        }
        stage('push image on dockerhub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')]) {
                    sh 'docker login -u yashpareek99 -p ${dockerhubpwd}'
                    sh 'docker push yashpareek99/nodejsapp'
                }
            }
        }
        stage('Deloyment on k8s') {
            steps {
              kubernetesDeploy (configs: 'deployment.yaml,service.yaml',kubeconfigId: 'k8spwd')
            }
        }
    }
}
