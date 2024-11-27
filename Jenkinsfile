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

                nexusArtifactUploader artifacts: [[artifactId: 'java-tomcat-example', classifier: '', file: 'target/java-tomcat-example.war', type: 'war']], credentialsId: 'ForNexus', groupId: 'com.example', nexusUrl: '3.111.219.116:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-repo-snapshot', version: '1.0-SNAPSHOT'
                    
                
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-credentials', path: '/manager', url: 'http://15.207.117.38:8080/manager')], contextPath: '/myapp', war: 'target/java-tomcat-example.war'
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