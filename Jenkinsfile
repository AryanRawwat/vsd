pipeline {
    agent any
    environment {
        MAVEN_HOME = tool name: 'Maven' // Ensure Maven is configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/AryanRawwat/vsd.git',
                    branch: 'main',
                    credentialsId: 'githubs'
                )
            }
        }
        stage('Build') {
            steps {
                echo 'Building the project...'
                sh "${MAVEN_HOME}/bin/mvn clean compile package"
            }
        }
        stage('Upload to Nexus') {
            steps {
                echo 'Uploading artifact to Nexus...'
                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: 'http://3.111.219.116:8081/',
                    repository: 'maven-repo', // Use "maven-releases" for non-SNAPSHOT versions
                    groupId: 'com.example',
                    version: '1.0-SNAPSHOT',
                    artifactId: 'java-tomcat-example',
                    credentialsId: 'ForNexus', // Replace with actual credentials ID in Jenkins
                    artifacts: [
                        [
                            artifactId: 'java-tomcat-example',
                            classifier: '',
                            file: 'target/java-tomcat-example-1.0-SNAPSHOT.war',
                            type: 'war'
                        ]
                    ]
                )
            }
        }
    }
    post {
        success {
            echo 'Build and upload completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}
