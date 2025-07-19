pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the GitHub Repository...'
            }
        }
        stage('Secret Scan (trufflehog)') {
            steps {
                echo 'Running TruffleHog scan...'
                // Run command when trufflehog is installed
                // sh 'trufflehog https://github.com/Akashsonawane571/devsecops-test.git'
            }
        }
        stage('Dependency Check') {
            steps {
                echo 'Running npm audit...'
                // sh 'npm audit' (for Node.js apps)
            }
        }
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                // sh 'docker build -t juice-shop .'
            }
        }
    }
}
