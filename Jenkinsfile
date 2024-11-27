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
        stage('Deploy to Nexus (Manual)') {
            steps {
                echo 'Manually deploying artifact to Nexus to create metadata...'
                sh """
                    ${MAVEN_HOME}/bin/mvn deploy:deploy-file \
                        -Durl=http://3.111.219.116:8081/repository/maven-repo-snapshot/ \
                        -DrepositoryId=nexus \
                        -Dfile=target/java-tomcat-example.war \
                        -DgroupId=com.example \
                        -DartifactId=java-tomcat-example \
                        -Dversion=1.0-SNAPSHOT \
                        -Dpackaging=war
                """
            }
        }
        stage('Upload to Nexus') {
            steps {
                echo 'Uploading artifact to Nexus...'

                nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: 'http://3.111.219.116:8081/repository/maven-repo-snapshot/',
                    repository: 'maven-repo-snapshot',  // Ensure correct repository for snapshots
                    credentialsId: 'ForNexus', // Replace with actual credentials ID in Jenkins
                    groupId: 'com.example',  // Ensure groupId is specified here
                    version: '1.0-SNAPSHOT', // Ensure version is specified here
                    artifacts: [
                        [
                            artifactId: 'java-tomcat-example',
                            file: 'target/java-tomcat-example.war',
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
