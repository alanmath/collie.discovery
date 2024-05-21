pipeline {
    agent any
    environment {
        K8S_PORT = 64606
        TARGET = 'aws'
    }
    stages {
        stage ('Jenkins Gateway') {
            steps {
                echo 'Gateway'
            }
        }
        stage ('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Image') {
            steps {
                script {
                    account = docker.build("alanmath/discovery:${env.BUILD_ID}", "-f Dockerfile .")
                }
            }
        }
        stage('Push Image'){
            steps{
                script {
                    docker.withRegistry('https://registry.hub.docker.com', '594871ef-08ca-47da-8365-6dd98ca976e6') {
                        account.push("${env.BUILD_ID}")
                        account.push("latest")
                    }
                }
            }
        }
        stage('Deploy on AWS k8s') {
            when {
                environment name: 'TARGET', value: 'aws'
            }
            steps {
                sh "kubectl apply -f ./k8s/deployment.yaml --validate=false"
                sh "kubectl apply -f ./k8s/service.yaml --validate=false"
            }
        }
    }
}
