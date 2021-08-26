pipeline {
    agent any
    tools {
        maven "Maven"
    }
    environment {
        NEXUS_VERSION = "nexus3.33"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "http://54.83.73.70:8081"
        NEXUS_REPOSITORY = "simpleapp-release"
        NEXUS_CREDENTIAL_ID = "1314288f-e205-445b-a56c-dce2cf3f28dd"
    }
    stages {
        stage("Maven Build") {
            steps {
                script {
                    bat 'mvn clean package'
                }
            }
        }
        stage("Deployment") {
            steps {
                script {
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'my-app', 
                            classifier: '', 
                            file: 'target/my-app-1.0.0.jar', 
                            type: 'jar'
                        ]
                    ], 
                    credentialsId: 'nexus', 
                    groupId: 'com.mycompany.app', 
                    nexusUrl: '54.83.73.70:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'simpleapp-release', 
                    version: '1.0.0'
                }
            }
        }
    }
}