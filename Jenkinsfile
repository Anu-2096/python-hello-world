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
        success {
            mail to: 'anushri20sept@gmail.com',
                 subject: "Jenkins: Build ${currentBuild.fullDisplayName} - Success",
                 body: "The build was successful!\nCheck the details at ${env.BUILD_URL}"
        }
        failure {
            mail to: 'anushri20sept@gmail.com',
                 subject: "Jenkins: Build ${currentBuild.fullDisplayName} - Failed",
                 body: "The build has failed.\nCheck the details at ${env.BUILD_URL}"
        }
    }
}
