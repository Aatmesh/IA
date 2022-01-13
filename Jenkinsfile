pipeline {
    agent any
    tools { 
        maven 'maven_test'
        jdk 'java_home'
    }
    stages {
        stage('SCM') {
            steps {
                script {
                    echo "Cloning the Code"
                    checkout([git branch: 'main', url: 'https://github.com/Aatmesh/IA'])
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    echo "PATH = ${PATH}"
                    echo "Printing Maven Home Path M2_HOME = ${M2_HOME}"
                    sh """
                        mvn clean install
                    """
                }
            }
        }
        stage('Jfrog Server') {
            steps {
                rtServer (
                    id: 'JFrog_Artifactory',
                    url: 'http://192.168.3.164:8082/artifactory',
                    username: 'aatmesh',
                    password: 'Admin@1234',
                    //credentialsId: 'ccrreeddeennttiiaall',
                    bypassProxy: true,
                    timeout: 300
                )
            }
        }
        stage('Upload') {
            steps {
                rtUpload (
                    serverId: 'JFrog_Artifactory',
                        spec: '''{
                            "files": [
                                {
                                "pattern": "*.war",
                                "target": "demo-libs-snapshot-local"
                                }
                            ]
                        }''',
                )
            }
        }
        stage('Publish Build Info') {
            steps {
                rtPublishBuildInfo (
                    serverId: 'JFrog_Artifactory'
                )
            }
        }
    }
}
