pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning the GitHub Repository.....'
            }
        }
        stage('Secret Scan (TruffleHog)') {
          steps {
            echo 'Running TruffleHog on latest commit only...'
            sh '''
              # Clone only latest commit
              git clone --depth=1 https://github.com/Akashsonawane571/devsecops-test.git temp_repo
              cd temp_repo
              # Run trufflehog locally on shallow clone
              trufflehog --regex --entropy=True --max_depth=10 . || true
              cd ..
              rm -rf temp_repo
            '''
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
