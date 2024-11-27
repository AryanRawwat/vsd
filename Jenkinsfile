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
                    credentialsId: '<nexus-credentials>', // Replace with actual credentials ID in Jenkins
                    artifacts: [
                        [
                            artifactId: 'java-tomcat-example',
                            groupId: 'com.example',
                            version: '1.0-SNAPSHOT',
                            file: 'target/java-tomcat-example.war', // Correct relative path
                            type: 'war',
                            classifier: '' // Optional, remove if not needed
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
