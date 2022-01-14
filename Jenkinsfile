pipeline {
    agent any
    tools {
        maven "maven-3.8.4"
    }
    stages {
        stage('clean and install') {
            steps {
               sh 'mvn clean install'
            }
        }
        stage('package') {
            steps {
               sh 'mvn package'
            }
        }
    }
}
