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
    }
    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs for details.'
        }
    }
}
