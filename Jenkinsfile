pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the application...'
                    sh '''
                        python3 -m venv venv | tee build.log
                        . venv/bin/activate | tee -a build.log
                        pip install -r requirements.txt | tee -a build.log  # Install requirements
                        pip install pytest flake8 bandit | tee -a build.log  # Ensure testing and analysis tools are installed
                        pip list | tee -a build.log  # Verify installed packages
                    '''
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit tests...'
                    sh '''
                        . venv/bin/activate | tee -a test.log  # Activate virtual environment
                        python -m pytest tests | tee -a test.log
                    '''
                }
            }
        }

        stage('Code Analysis') {
            steps {
                script {
                    echo 'Running code analysis...'
                    sh '''
                        . venv/bin/activate | tee -a analysis.log  # Activate virtual environment
                        python -m flake8 app || true | tee -a analysis.log  # Ignore flake8 exit code
                    '''
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    echo 'Running security scan...'
                    sh '''
                        . venv/bin/activate | tee -a security.log  # Activate virtual environment
                        python -m bandit -r app | tee -a security.log
                    '''
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging...'
                    // Add staging deployment commands here
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    sh '''
                        . venv/bin/activate | tee -a staging_test.log  # Activate virtual environment
                        python -m pytest tests/ | tee -a staging_test.log
                    '''
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production...'
                    // Add production deployment commands here
                }
            }
        }
    }

    post {
        always {
            // Archive the logs for attaching to the email
            archiveArtifacts artifacts: '*.log', allowEmptyArchive: true
        }
        success {
            emailext(
                to: 'anushri20sept@gmail.com',
                subject: "Jenkins: Build ${currentBuild.fullDisplayName} - SUCCESS",
                body: """The build was successful!
                        Check the details at ${env.BUILD_URL}
                        Attached are the logs for reference.""",
                attachmentsPattern: '**/*.log', // Attach all log files
                attachLog: true // Optionally include the console log
            )
        }
        failure {
            emailext(
                to: 'anushri20sept@gmail.com',
                subject: "Jenkins: Build ${currentBuild.fullDisplayName} - FAILURE",
                body: """The build failed.
                        Check the details at ${env.BUILD_URL}
                        Attached are the logs for further investigation.""",
                attachmentsPattern: '**/*.log', // Attach all log files
                attachLog: true // Optionally include the console log
            )
        }
    }
}
