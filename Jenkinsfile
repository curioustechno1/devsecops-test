pipeline {
    agent any

    environment {
        DEPENDENCY_CHECK  = '/home/rocky1/tools/dependency-check/bin/dependency-check.sh'
        SONAR_SCANNER     = tool name: 'sonar-scanner'
        ZAP_REPORT_HTML   = 'zap_report.html'
        ZAP_REPORT_XML    = 'zap_report.xml'
        ZAP_REPORT_JSON   = 'zap_report.json'
        IP                = '13.53.137.137'
        EC2_HOST          = "ubuntu@${IP}"
        EC2_APP_PORT      = '3000'
        EC2_KEY_ID        = 'ec2-ssh-key'
    }

    stages {

        stage('Deploy App to AWS EC2') {
            steps {
                echo '🚀 Deploying Juice Shop to EC2...'
                sshagent(credentials: [env.EC2_KEY_ID]) {
                    sh """#!/bin/bash
                        ssh -o StrictHostKeyChecking=no $EC2_HOST "docker rm -f juice-shop || true"
                        ssh $EC2_HOST "docker pull kumar0ndocker/my-juice-shop:v1"
                        ssh $EC2_HOST "docker run -d --name juice-shop -p $EC2_APP_PORT:$EC2_APP_PORT kumar0ndocker/my-juice-shop:v1"
                        sleep 20
                    """
                }
            }
        }
    /*
        stage('Run ZAP on EC2') {
            steps {
                echo '🛡️ Running OWASP ZAP DAST scan on EC2...'
                sshagent(credentials: [env.EC2_KEY_ID]) {
                    sh """#!/bin/bash
                        ssh $EC2_HOST "docker run --rm -v /home/ubuntu:/zap/wrk zaproxy/zap-stable \
                          zap-baseline.py -t http://$IP:$EC2_APP_PORT \
                          -r $ZAP_REPORT_HTML -x $ZAP_REPORT_XML -J $ZAP_REPORT_JSON || true"

                        scp $EC2_HOST:/home/ubuntu/$ZAP_REPORT_HTML .
                        scp $EC2_HOST:/home/ubuntu/$ZAP_REPORT_XML .
                        scp $EC2_HOST:/home/ubuntu/$ZAP_REPORT_JSON .
                    """
                }
                archiveArtifacts artifacts: "$ZAP_REPORT_HTML, $ZAP_REPORT_XML, $ZAP_REPORT_JSON", onlyIfSuccessful: false
            }
        }
*/
     /*   stage('Run Nikto on EC2') {
            steps {
                echo '🔍 Running Nikto scan on EC2...'
                timeout(time: 12, unit: 'MINUTES') {
                    sshagent(credentials: [env.EC2_KEY_ID]) {
                        sh """#!/bin/bash
                            ssh $EC2_HOST '
                                rm -rf nikto
                                git clone https://github.com/sullo/nikto.git
                                cd nikto/program
                                chmod +x nikto.pl
                                ./nikto.pl -h http://$IP:$EC2_APP_PORT:3000 -Tuning 123456789abcde \
                                -Plugins "apacheexpect,backups,cgis,favicon,headers,httpoptions,misc,robots,ssl,svn,wiki" \
                                -o $WORKSPACE/$NIKTO_REPORT -Format html || true

                              
                            '
                            scp $EC2_HOST:/home/ubuntu/nikto_report.html .
                        """
                    }
                }
                archiveArtifacts artifacts: 'nikto_report.html', onlyIfSuccessful: false
            }
        }*/
            stage('Run Nikto DAST Scan') {
                steps {
                    echo 'Running Nikto DAST Scan...'
                    sh '''
                        rm -rf nikto
                        git clone https://github.com/sullo/nikto.git
                        cd nikto/program
                        chmod +x nikto.pl
                        ./nikto.pl -h $TARGET_URL -o $WORKSPACE/$NIKTO_REPORT -Format html || true
                    '''
                    archiveArtifacts artifacts: "${NIKTO_REPORT}", onlyIfSuccessful: false
                }
            }
    }
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

        stage('Secret Scan (TruffleHog)') {
            steps {
                echo 'Running TruffleHog on latest commit only...'
                sh '''
                  # Clone only latest commit
                  cd temp_repo
                  # Run trufflehog locally on shallow clone
                  trufflehog --regex --entropy=True --max_depth=10 . > ../trufflehog_report.json || true
                  cd ..
                '''
                archiveArtifacts artifacts: 'trufflehog_report.json', onlyIfSuccessful: false
          }
        }

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
        
        stage('SonarQube Scan') {
            steps {
                echo 'Starting SonarQube SAST Scan...'
                withSonarQubeEnv('sonarqube') {
                    withCredentials([string(credentialsId: 'newtoken', variable: 'SONAR_TOKEN')]) {
                        sh '''
                            cd temp_repo
                            $SONAR_SCANNER/bin/sonar-scanner \
                              -Dsonar.projectKey=devsecops-test \
                              -Dsonar.sources=. \
                              -Dsonar.host.url=http://localhost:9000 \
                              -Dsonar.token=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }

        stage('Build Project') {
            steps {
                echo 'Building the Java project with Maven...'
                dir('temp_repo') {
                    sh 'mvn clean install -DskipTests'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                dir('temp_repo') {
                    sh 'docker build -t juice-shop .'
                }
            }
        } */

    /*    stage('Deploy to Server') {
            steps {
                timeout(time: 3, unit: 'MINUTES') {
                    sshagent(credentials: ['app-server']) {
                        sh '''
                            scp -o StrictHostKeyChecking=no temp_repo/webgoat-server/target/webgoat-server-v8.2.0-SNAPSHOT.jar ubuntu@3.109.152.116:/WebGoat
                            ssh -o StrictHostKeyChecking=no ubuntu@3.109.152.116 "nohup java -jar /WebGoat/webgoat-server-v8.2.0-SNAPSHOT.jar > /dev/null 2>&1 &"
                        '''
                    }
                }
            }
        } 
        */
        /*
        stage('Run ZAP DAST Scan (Baseline)') {
            steps {
                echo 'Running ZAP Baseline DAST Scan...'
                sh '''
                    docker run --rm \
                      -v $WORKSPACE:/zap/wrk/:rw \
                      zaproxy/zap-stable \
                      zap-baseline.py -t $TARGET_URL \
                      -r $ZAP_REPORT_HTML -x $ZAP_REPORT_XML -J $ZAP_REPORT_JSON || true
                '''
                archiveArtifacts artifacts: "${ZAP_REPORT_HTML}, ${ZAP_REPORT_XML}, ${ZAP_REPORT_JSON}", onlyIfSuccessful: false
            }
        }
    }
    */
    post {
        always {
            echo 'Cleaning up temporary files...'
            sh '''
                rm -rf temp_repo dependency-check-report trufflehog_report.txt \
                       $ZAP_REPORT_HTML $ZAP_REPORT_XML $ZAP_REPORT_JSON || true
            '''
        }
    }
}
