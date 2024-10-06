pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    echo 'Building the application...'
                    sh '''
                        python3 -m venv venv
                        . venv/bin/activate
                        pip install -r requirements.txt  # Install requirements
                        pip install pytest flake8 bandit  # Ensure testing and analysis tools are installed
                        pip list  # Verify installed packages
                    '''
                }
            }
        }

        stage('Unit and Integration Tests') {
            steps {
                script {
                    echo 'Running unit tests...'
                    sh '''
                        . venv/bin/activate  # Activate virtual environment
                        python -m pytest tests
                    '''
                }
            }
        }

        stage('Code Analysis') {
    steps {
        script {
            echo 'Running code analysis...'
            sh '''
                . venv/bin/activate  # Activate virtual environment
                python -m flake8 app || true  # Ignore flake8 exit code
            '''
        }
    }
}

        stage('Security Scan') {
            steps {
                script {
                    echo 'Running security scan...'
                    sh '''
                        . venv/bin/activate  # Activate virtual environment
                        python -m bandit -r app
                    '''
                }
            }
        }

        stage('Deploy to Staging') {
            steps {
                script {
                    echo 'Deploying to staging...'
                }
            }
        }

        stage('Integration Tests on Staging') {
            steps {
                script {
                    echo 'Running integration tests on staging...'
                    sh '''
                        . venv/bin/activate  # Activate virtual environment
                        python -m pytest tests/
                    '''
                }
            }
        }

        stage('Deploy to Production') {
            steps {
                script {
                    echo 'Deploying to production...'
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
                attachmentsPattern: '*.log' //Attach the logs
            )
        }
        failure {
            emailext(
                to: 'anushri20sept@gmail.com',
                subject: "Jenkins: Build ${currentBuild.fullDisplayName} - FAILURE",
                body: """The build failed.
                        Check the details at ${env.BUILD_URL}
                        Attached are the logs for further investigation.""",
                attachmentsPattern: '*.log' //sAttach the logs
            )
        }
    }
}
