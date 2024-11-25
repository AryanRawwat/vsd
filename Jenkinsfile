pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                // Checkout the GitHub repository
                git branch: 'main', 
                    git branch: 'main', credentialsId: 'jenkins-github', url: 'https://github.com/AryanRawwat/vsd.git'
                
                // Print the contents of the repository
                script {
                    sh 'ls -la'
                }
            }
        }
    }
}