def artifact_name = 'target/my-app-1.0.0.jar'
def bucket_name = "nexus-nbs"
def aws_credentials_id = 'aws'
def aws_region = 'us-east-1'
pipeline {
    agent any
    tools {
        maven "Maven"
    }
    environment {
        NEXUS_VERSION = "nexus3.33"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "http://54.242.112.97:8081"
        NEXUS_REPOSITORY = "simpleapp-release"
        NEXUS_CREDENTIAL_ID = "1314288f-e205-445b-a56c-dce2cf3f28dd"
    }
    stages {
        stage("Build") {
            steps {
                script {
                    bat 'mvn clean package'
                }
            }
        }
        stage("Artifact Upload To Nexus") {
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
                    nexusUrl: '54.242.112.97:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'simpleapp-release', 
                    version: '1.0.0'
                }
            }
        }
        stage("Deploying To S3") {
            input {
                message "Wants To Deploy to Production?"
            }
            steps {
                withEnv([
                "AWS_SESSION_TOKEN=${env.AWS_SESSION_TOKEN}"]) {
                    withAWS(region: "${aws_region}", credentials: "${aws_credentials_id}") {
                    s3Upload(bucket: "${bucket_name}", file: "${artifact_name}");
                    }
                }
                    
            }
        }
    }
}