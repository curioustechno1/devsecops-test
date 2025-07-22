pipeline {
    agent any

    environment {
        DEPENDENCY_CHECK = '/opt/dependency-check/dependency-check/bin/dependency-check.sh'
    }

    stages {

        /*
        stage('Clone Repository') {
            steps {
                echo 'Cloning the GitHub Repository...'
                sh '''
                    rm -rf temp_repo
                    git clone --depth=1 https://github.com/Akashsonawane571/devsecops-test.git temp_repo
                '''
            }
        }
        */

        /*
        stage('Dependency Check (OWASP)') {
            steps {
                echo 'Running OWASP Dependency-Check...'
                sh '''
                    mkdir -p dependency-check-report
                    cd temp_repo
                    $DEPENDENCY_CHECK --project "Universal-SCA-Scan" --scan . --format ALL --out ../dependency-check-report || true
                    cd ..
                '''
                archiveArtifacts artifacts: 'dependency-check-report/*', onlyIfSuccessful: false
            }
        }
        */

        stage('SonarQube Scan') {
            steps {
                echo 'Starting SonarQube SAST Scan...'
                withSonarQubeEnv('sonarqube') {
                    withCredentials([string(credentialsId: 'sonarqube-token', variable: 'SONAR_TOKEN')]) {
                        sh '''
                            rm -rf temp_repo
                            git clone --depth=1 https://github.com/Akashsonawane571/devsecops-test.git temp_repo
                            cd temp_repo
                            sonar-scanner \
                              -Dsonar.projectKey=devsecops-test \
                              -Dsonar.sources=. \
                              -Dsonar.host.url=$SONARQUBE_URL \
                              -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }

        /*
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                // sh 'docker build -t juice-shop .'
            }
        }
        */
    }

    post {
        always {
            echo 'Cleaning up temporary files...'
            sh 'rm -rf temp_repo dependency-check-report trufflehog_report.txt || true'
        }
    }
}

