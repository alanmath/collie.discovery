pipeline {
    agent any
    stages {
        stage ('Jenkins Discovery') {
            steps {
                echo 'Discovery'
            }
        }
        stage ('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
    }
}